---
tags:
  - 前端
  - React
  - AntDesign
  - AntDesignPro
  - hooks
  - BLOG
date: 2025-09-01
---
标题：【React+Ant Design Pro】关于文件上传通用hooks

适用于单文件。（待扩展为多文件）

# Hooks

```ts
/*
 * @Author: Jessica Wang 1271736670@qq.com
 * @Date: 2025-08-19 13:42:55
 * @LastEditors: Jessica Wang 1271736670@qq.com
 * @LastEditTime: 2025-08-20 10:40:46
 * @FilePath: \official-admin-frontend\src\hooks\useFileUpload.ts
 * @Description: 自定义文件上传Hook，支持多种文件类型和状态管理
 */
import { getFileInfo, removeFile } from '@/services/ant-design-pro/file';
import { getFileUrl } from '@/utils/file';
import { App } from 'antd';
import { useState } from 'react';

type FileType = 'image' | 'document'; // 可扩展的文件类型

interface FileItem {
  uid: string;
  uuid: string;
  name: string;
  status: 'done' | 'uploading' | 'error';
  url?: string;
  response?: {
    code: number;
    data: { uuid: string };
  };
}

interface FileState {
  fileList: FileItem[]; // 当前文件列表
  originalFileList: FileItem[]; // 原始文件列表（用于判断新上传的文件）
  deleteFileList: FileItem[]; // 待删除文件列表（用于存储需要删除的文件）
}

interface FileUploadConfig {
  getFileUrl?: (uuid: string) => string; // 自定义文件URL生成
  defaultFileType?: FileType; // 默认文件类型
}

export const useFileUpload = (config: FileUploadConfig = {}) => {
  const { message } = App.useApp();
  const { getFileUrl: customGetFileUrl, defaultFileType = 'image' } = config;

  // 所有文件类型的状态
  const [fileStates, setFileStates] = useState<Record<FileType, FileState>>({
    image: { fileList: [], originalFileList: [], deleteFileList: [] },
    document: { fileList: [], originalFileList: [], deleteFileList: [] },
  });

  // 获取文件URL的通用方法
  const resolveFileUrl = (uuid: string, type: FileType) => {
    return customGetFileUrl?.(uuid) || (type === 'image' ? getFileUrl(uuid) : undefined);
  };

  // 更新文件状态
  const updateFileState = (
    type: FileType,
    newState: Partial<FileState> | ((prev: FileState) => Partial<FileState>),
  ) => {
    setFileStates((prev) => ({
      …prev,
      [type]: {
        …prev[type],
        …(typeof newState === 'function' ? newState(prev[type]) : newState),
      },
    }));
  };

  // 加载文件列表
  const loadFiles = async (
    type: FileType = defaultFileType,
    form: any,
    fieldName: string,
    uuids?: string[],
  ) => {
    if (!uuids?.length) {
      updateFileState(type, { fileList: [], originalFileList: [] });
      return;
    }

    try {
      const fileInfos = await Promise.all(uuids.map((uuid) => getFileInfo(uuid)));
      const files = fileInfos
        .filter((info) => info.data?.uuid) // 过滤掉无效文件
        .map((info) => {
          const uuid = info.data.uuid!;
          form.setFieldsValue({ [fieldName]: uuid });
          return {
            uid: uuid,
            uuid: uuid,
            name: info.data.name || '未命名文件',
            status: 'done' as const,
            url: resolveFileUrl(uuid, type),
            response: {
              code: 0,
              data: { uuid },
            },
          } satisfies FileItem; // 确保符合类型
        });

      updateFileState(type, {
        fileList: files,
        originalFileList: files,
        deleteFileList: [],
      });
    } catch (error) {
      console.error(`加载${type}文件失败:`, error);
      updateFileState(type, { fileList: [], originalFileList: [] });
    }
  };

  // 判断文件是否为新上传
  const isNewFile = (type: FileType, file: FileItem) => {
    return !fileStates[type].originalFileList.some(
      (original) =>
        (original.response?.data.uuid || original.uid) === (file.response?.data.uuid || file.uid),
    );
  };

  // 通用文件删除函数
  const deleteFiles = async (files: FileItem[]) => {
    const uuids = files
      .map((f) => f.response?.data?.uuid || f.uid)
      .filter((uuid): uuid is string => !!uuid);

    await Promise.all(
      uuids.map((uuid) => removeFile(uuid).catch((e) => console.error(`删除文件失败: ${uuid}`, e))),
    );
  };

  // 删除新上传的文件
  const cancelUploads = async (type: FileType, preserveUuids: string[] = []) => {
    try {
      const { fileList, deleteFileList } = fileStates[type];
      const filesToDelete = […fileList, …deleteFileList]
        .filter((file) => isNewFile(type, file))
        .filter((file) => !preserveUuids.includes(file.response?.data?.uuid || file.uid));

      await deleteFiles(filesToDelete);
      updateFileState(type, { deleteFileList: [] }); // 清空删除列表
    } catch (error) {
      console.error('取消上传时出错:', error);
      message.error('取消上传时出错');
    }
  };

  // 重置文件状态
  const resetFileState = (type: FileType = defaultFileType) => {
    updateFileState(type, {
      fileList: [],
      originalFileList: [],
      deleteFileList: [],
    });
  };

  // 添加文件到删除列表
  const addToDeleteList = (type: FileType, files: FileItem[]) => {
    updateFileState(type, (prev) => ({
      deleteFileList: […prev.deleteFileList, …files],
    }));
  };

  // 从删除列表中移除文件
  const removeFromDeleteList = (type: FileType, fileUuids: string[]) => {
    updateFileState(type, (prev) => ({
      deleteFileList: prev.deleteFileList.filter(
        (file) => !fileUuids.includes(file.response?.data?.uuid || file.uid),
      ),
    }));
  };

  // 清空删除列表
  const clearDeleteList = (type: FileType = defaultFileType) => {
    updateFileState(type, { deleteFileList: [] });
  };

  // 更新原始文件列表
  const updateOriginalFileList = (type: FileType, files: FileItem[]) => {
    updateFileState(type, { originalFileList: files });
  };

  // 添加文件到原始文件列表
  const addToOriginalFileList = (type: FileType, files: FileItem[]) => {
    updateFileState(type, (prev) => ({
      originalFileList: […prev.originalFileList, …files],
    }));
  };

  // 从原始文件列表中移除文件
  const removeFromOriginalFileList = (type: FileType, fileUuids: string[]) => {
    updateFileState(type, (prev) => ({
      originalFileList: prev.originalFileList.filter(
        (file) => !fileUuids.includes(file.response?.data?.uuid || file.uid),
      ),
    }));
  };

  // 设置文件列表
  const setFileList = (type: FileType, files: FileItem[]) => {
    updateFileState(type, { fileList: files });
  };

  // 添加文件到当前文件列表
  const addToFileList = (type: FileType, files: FileItem[]) => {
    updateFileState(type, (prev) => ({
      fileList: […prev.fileList, …files],
    }));
  };

  // 从当前文件列表中移除文件
  const removeFromFileList = (type: FileType, fileUuids: string[]) => {
    updateFileState(type, (prev) => ({
      fileList: prev.fileList.filter(
        (file) => !fileUuids.includes(file.response?.data?.uuid || file.uid),
      ),
    }));
  };

  // 获取特定类型文件的控制器
  const getFileController = (type: FileType = defaultFileType) => ({
    …fileStates[type],

    // 文件列表操作
    setFileList: (files: FileItem[]) => setFileList(type, files),
    addToFileList: (files: FileItem[]) => addToFileList(type, files),
    removeFromFileList: (fileUuids: string[]) => removeFromFileList(type, fileUuids),

    // 原始文件列表操作
    updateOriginalFileList: (files: FileItem[]) => updateOriginalFileList(type, files),
    addToOriginalFileList: (files: FileItem[]) => addToOriginalFileList(type, files),
    removeFromOriginalFileList: (fileUuids: string[]) =>
      removeFromOriginalFileList(type, fileUuids),

    // 待删除列表操作
    addToDeleteList: (files: FileItem[]) => addToDeleteList(type, files),
    removeFromDeleteList: (fileUuids: string[]) => removeFromDeleteList(type, fileUuids),
    clearDeleteList: () => clearDeleteList(type),

    // 其他操作
    loadFiles: (form: any, fieldName: string, uuids?: string[]) =>
      loadFiles(type, form, fieldName, uuids), // 加载文件
    isNewFile: (file: FileItem) => isNewFile(type, file), // 判断文件是否为新上传的
    cancelUploads: (preserveUuids?: string[]) => cancelUploads(type, preserveUuids), // 删除新上传的文件
    deleteFiles: (files: FileItem[]) => deleteFiles(files), // 通用文件删除函数
    reset: () => resetFileState(type), // 重置文件状态
  });

  return {
    // 默认文件类型的控制器
    …getFileController(),

    // 其他文件类型的控制器
    image: getFileController('image'),
    document: getFileController('document'),

    // 原始状态（高级用法）
    fileStates,
  };
};
```

# 使用方法

```tsx
<ProFormUploadButton
	colProps={{ md: 24, xl: 12 }}
	icon={false}
	name="logo"
	label="LOGO"
	rules={[{ required: true, message: '请上传LOGO' }]}
	fileList={image.fileList}
	max={FILE_CONFIG.LOGO.maxCount}
	title={
	  <div style={{ display: 'flex', flexDirection: 'column', alignItems: 'center' }}>
		<span>点击上传图片</span>
		<span style={{ fontSize: 12, color: '#999' }} className={style.uploadInfo}>
		  {`（${FILE_CONFIG.LOGO.acceptTypes.join('、')}格式，大小不超过${
			FILE_CONFIG.LOGO.maxSize / 1024 / 1024
		  }MB）`}
		</span>
	  </div>
	}
	fieldProps={{
	  name: 'logo',
	  listType: 'picture-card',
	  style: { width: '200px', height: '200px' },
	  className: style.uploadButton, // 使用自定义样式
	  accept: FILE_CONFIG.LOGO.acceptTypes.join(', '),
	  beforeUpload: (file) => beforeUpload(file, FILE_CONFIG.LOGO, 'image'),
	  customRequest: (options) => customRequest(options, 'cover', form, 'logoUuid'),
	  onChange: (info) => onChange(info, image.setFileList, form, 'logoUuid'),
	  onRemove: (file) => onRemove(file, image.addToDeleteList, form, 'logoUuid', 'image'),
	}}
/>
```