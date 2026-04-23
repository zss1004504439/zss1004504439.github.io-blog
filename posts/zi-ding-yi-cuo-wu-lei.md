---
id: VNcBGM
title: 自定义错误类
createdAt: "2025-04-23 15:06:18"
updated: "2026-04-23 18:53:10"
tags: []
tag_ids: []
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

```typescript
// errors/base.ts
export class AppError extends Error {
  public readonly statusCode: number;
  public readonly isOperational: boolean;
  public readonly code: string;

  constructor(message: string, statusCode: number, code: string) {
    super(message);
    this.statusCode = statusCode;
    this.isOperational = true; // 可预期的业务错误
    this.code = code;
    Error.captureStackTrace(this, this.constructor);
  }
}

// 具体错误类
export class NotFoundError extends AppError {
  constructor(resource: string, id?: string | number) {
    const msg = id ? `${resource} (id: ${id}) 不存在` : `${resource}不存在`;
    super(msg, 404, 'NOT_FOUND');
  }
}

export class ValidationError extends AppError {
  public readonly errors: Array<{ field: string; message: string }>;

  constructor(errors: Array<{ field: string; message: string }>) {
    super('请求参数验证失败', 422, 'VALIDATION_ERROR');
    this.errors = errors;
  }
}

export class UnauthorizedError extends AppError {
  constructor(message = '请先登录') {
    super(message, 401, 'UNAUTHORIZED');
  }
}

export class ForbiddenError extends AppError {
  constructor(message = '无权访问') {
    super(message, 403, 'FORBIDDEN');
  }
}

export class ConflictError extends AppError {
  constructor(message: string) {
    super(message, 409, 'CONFLICT');
  }
}
```