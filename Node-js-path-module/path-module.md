# 📘 Node.js Path Module - Complete TypeScript Guide

**Documentation by:** Md. Nazim Uddin  
**Email:** nazimmuddin10@gmail.com  
**Reference:** [Official Node.js Path Module Documentation](https://nodejs.org/api/path.html)

> **Note:** This guide is based on the official Node.js documentation. For the most up-to-date and comprehensive information, always refer to the [official Node.js path module documentation](https://nodejs.org/api/path.html).


## Table of Contents
1. [Introduction](#1-introduction)
2. [Global Variables](#2-global-variables)
3. [Path Composition Methods](#3-path-composition-methods)
4. [Path Decomposition Methods](#4-path-decomposition-methods)
5. [Path Information Methods](#5-path-information-methods)
6. [Constants](#6-constants)
7. [Platform-Specific Methods](#7-platform-specific-methods)
8. [Practical Examples](#8-practical-examples)
9. [Summary Table](#9-summary-table)

## Quick Reference Table

| Method/Constant | Description | Type Signature |
|----------------|-------------|----------------|
| `__dirname` | Current directory path | `string` |
| `__filename` | Current file path | `string` |
| `path.join()` | Join path segments | `(...paths: string[]): string` |
| `path.resolve()` | Resolve to absolute path | `(...paths: string[]): string` |
| `path.normalize()` | Normalize path | `(path: string): string` |
| `path.parse()` | Parse path into object | `(path: string): ParsedPath` |
| `path.format()` | Format object to path | `(pathObject: FormatInputPathObject): string` |
| `path.basename()` | Get filename | `(path: string, ext?: string): string` |
| `path.dirname()` | Get directory name | `(path: string): string` |
| `path.extname()` | Get file extension | `(path: string): string` |
| `path.isAbsolute()` | Check if absolute path | `(path: string): boolean` |
| `path.relative()` | Get relative path | `(from: string, to: string): string` |
| `path.sep` | Path separator | `string` |
| `path.delimiter` | Path delimiter | `string` |
| `path.win32` | Windows path methods | `object` |
| `path.posix` | POSIX path methods | `object` |
| `path.toNamespacedPath()` | Convert to namespaced path | `(path: string): string` |

---

## 1. Introduction

The `path` module in Node.js provides utilities for working with file and directory paths. This guide includes TypeScript type definitions and examples.

```typescript
// Importing the path module with TypeScript
import * as path from 'path';

// Type definitions for path components
interface PathComponents {
  root: string;
  dir: string;
  base: string;
  ext: string;
  name: string;
}

console.log("=== Node.js Path Module TypeScript Guide ===");
console.log("Path module imported successfully");
```

---

## 2. Global Variables

### 2.1 `__dirname`
Returns the directory name of the current module as a string.

```typescript
console.log("\n--- __dirname ---");
console.log("Current directory:", __dirname);
// Output example: /Users/username/projects

// TypeScript knows this is a string
const currentDir: string = __dirname;
console.log("Type of __dirname:", typeof currentDir);
// Output: string
```

### 2.2 `__filename`
Returns the filename of the current module as a string.

```typescript
console.log("\n--- __filename ---");
console.log("Current file:", __filename);
// Output example: /Users/username/projects/app.ts

// Type annotation
const currentFile: string = __filename;
console.log("Type of __filename:", typeof currentFile);
// Output: string
```

**Real-world Example:**
```typescript
console.log("\n--- Real-world Example: Config Path ---");
// Configuring a server with paths relative to current directory
const configPath: string = path.join(__dirname, 'config', 'server.json');
console.log("Config file located at:", configPath);
// Output example: /Users/username/projects/config/server.json
```

---

## 3. Path Composition Methods

### 3.1 `path.join(...paths: string[]): string`
Joins all given path segments together using the platform-specific separator.

```typescript
console.log("\n--- path.join() ---");
// Simple Example
const fullPath: string = path.join('users', 'docs', 'file.txt');
console.log("Joined path:", fullPath);
// Output: users/docs/file.txt (Linux/Mac) or users\docs\file.txt (Windows)

// Real-world Example: Building API endpoint paths
const apiPath: string = path.join('/api', 'v1', 'users', 'profile');
console.log("API Endpoint:", apiPath);
// Output: /api/v1/users/profile
```

### 3.2 `path.resolve(...paths: string[]): string`
Resolves a sequence of paths or path segments into an absolute path.

```typescript
console.log("\n--- path.resolve() ---");
// Simple Example
const absolutePath: string = path.resolve('src', 'app.ts');
console.log("Resolved absolute path:", absolutePath);
// Output example: /home/user/project/src/app.ts

// Real-world Example: Resolving project root paths
const projectRoot: string = path.resolve(__dirname, '..');
const assetsPath: string = path.resolve(projectRoot, 'assets', 'images');
console.log("Project root:", projectRoot);
console.log("Assets directory:", assetsPath);
// Output example: Assets directory: /home/user/project/assets/images
```

### 3.3 `path.normalize(path: string): string`
Normalizes the given path, resolving '..' and '.' segments.

```typescript
console.log("\n--- path.normalize() ---");
// Simple Example
const cleanPath: string = path.normalize('docs/../src/./app//utils');
console.log("Normalized path:", cleanPath);
// Output: src/app/utils

// Real-world Example: Cleaning user input paths with TypeScript type safety
function safePath(userInput: string): string {
  const normalized: string = path.normalize(userInput);
  if (normalized.startsWith('..')) {
    throw new Error('Invalid path: Cannot access parent directories');
  }
  return normalized;
}

try {
  const safeResult = safePath('files/../docs/./report.txt');
  console.log("Safe path result:", safeResult);
  // Output: docs/report.txt
} catch (error) {
  console.error("Error:", error.message);
}
```

---

## 4. Path Decomposition Methods

### 4.1 `path.parse(path: string): PathComponents`
Returns an object whose properties represent significant elements of the path.

```typescript
console.log("\n--- path.parse() ---");
// Simple Example
const parsed: path.ParsedPath = path.parse('/home/user/docs/report.pdf');
console.log("Parsed path object:", parsed);
/* Output:
{
  root: '/',
  dir: '/home/user/docs',
  base: 'report.pdf',
  ext: '.pdf',
  name: 'report'
}
*/

// Real-world Example: File upload processing with TypeScript interface
interface UploadInfo {
  originalName: string;
  filename: string;
  uploadDir: string;
  extension: string;
}

function processUpload(filePath: string): UploadInfo {
  const { name, ext, dir }: path.ParsedPath = path.parse(filePath);
  
  return {
    originalName: filePath,
    filename: `${name}-${Date.now()}${ext}`,
    uploadDir: path.join(dir, 'uploads'),
    extension: ext
  };
}

const uploadInfo = processUpload('/tmp/docs/report.pdf');
console.log("Upload information:", uploadInfo);
/* Output:
{
  originalName: '/tmp/docs/report.pdf',
  filename: 'report-1633024567890.pdf',
  uploadDir: '/tmp/docs/uploads',
  extension: '.pdf'
}
*/
```

### 4.2 `path.format(pathObject: path.FormatInputPathObject): string`
Returns a path string from an object (opposite of `path.parse()`).

```typescript
console.log("\n--- path.format() ---");
// Simple Example
const pathObj: path.FormatInputPathObject = {
  dir: '/home/user/docs',
  name: 'report',
  ext: '.pdf'
};
const formattedPath: string = path.format(pathObj);
console.log("Formatted path:", formattedPath);
// Output: /home/user/docs/report.pdf

// Real-world Example: Generating download file paths
function createDownloadPath(userId: string, filename: string): string {
  const { name, ext }: path.ParsedPath = path.parse(filename);
  
  return path.format({
    dir: path.join('/downloads', userId),
    name: `${name}-${Date.now()}`,
    ext: ext
  });
}

const downloadPath = createDownloadPath('user123', 'document.docx');
console.log("Download path:", downloadPath);
// Output: /downloads/user123/document-1633024567890.docx
```

### 4.3 `path.basename(path: string, ext?: string): string`
Returns the last portion of a path.

```typescript
console.log("\n--- path.basename() ---");
// Simple Example
const filename: string = path.basename('/docs/api/user.json');
console.log("Filename with extension:", filename);
// Output: user.json

const nameWithoutExt: string = path.basename('/docs/api/user.json', '.json');
console.log("Filename without extension:", nameWithoutExt);
// Output: user

// Real-world Example: File processing with proper typing
function processFile(filePath: string): string {
  const extension: string = path.extname(filePath);
  const filename: string = path.basename(filePath, extension);
  
  console.log(`Processing ${extension} file: ${filename}`);
  return filename;
}

const processedName = processFile('/data/users.json');
// Output: Processing .json file: users
console.log("Processed filename:", processedName);
// Output: users
```

### 4.4 `path.dirname(path: string): string`
Returns the directory name of a path.

```typescript
console.log("\n--- path.dirname() ---");
// Simple Example
const directory: string = path.dirname('/docs/api/user.json');
console.log("Directory path:", directory);
// Output: /docs/api

// Real-world Example: Creating backup directories
function createBackup(filePath: string): string {
  const dir: string = path.dirname(filePath);
  const backupDir: string = path.join(dir, 'backups');
  
  console.log(`Creating backup in ${backupDir}`);
  return backupDir;
}

const backupDir = createBackup('/data/config.json');
// Output: Creating backup in /data/backups
console.log("Backup directory:", backupDir);
// Output: /data/backups
```

### 4.5 `path.extname(path: string): string`
Returns the extension of the path.

```typescript
console.log("\n--- path.extname() ---");
// Simple Example
const extension1: string = path.extname('document.pdf');
console.log("Extension of document.pdf:", extension1);
// Output: .pdf

const extension2: string = path.extname('archive.tar.gz');
console.log("Extension of archive.tar.gz:", extension2);
// Output: .gz

// Real-world Example: File type validation with TypeScript
function isValidFileType(filePath: string, allowedTypes: string[]): boolean {
  const extension: string = path.extname(filePath).toLowerCase();
  return allowedTypes.includes(extension);
}

const allowed = ['.jpg', '.jpeg', '.png', '.gif'];
console.log("Is photo.jpg a valid image?", isValidFileType('photo.jpg', allowed));
// Output: true
console.log("Is document.pdf a valid image?", isValidFileType('document.pdf', allowed));
// Output: false
```

---

## 5. Path Information Methods

### 5.1 `path.isAbsolute(path: string): boolean`
Determines if path is an absolute path.

```typescript
console.log("\n--- path.isAbsolute() ---");
// Simple Example
const isAbsolute1: boolean = path.isAbsolute('/home/user/file.txt');
console.log("Is '/home/user/file.txt' absolute?", isAbsolute1);
// Output: true

const isAbsolute2: boolean = path.isAbsolute('docs/file.txt');
console.log("Is 'docs/file.txt' absolute?", isAbsolute2);
// Output: false

// Real-world Example: Path validation with TypeScript
function validateFilePath(userInput: string): string {
  if (path.isAbsolute(userInput)) {
    throw new Error('Absolute paths are not allowed');
  }
  
  return path.normalize(userInput);
}

try {
  const safePath = validateFilePath('docs/reports/summary.txt');
  console.log("Valid path:", safePath);
  // Output: docs/reports/summary.txt
} catch (error) {
  console.error("Error:", error.message);
}
```

### 5.2 `path.relative(from: string, to: string): string`
Returns the relative path from 'from' to 'to'.

```typescript
console.log("\n--- path.relative() ---");
// Simple Example
const relativePath: string = path.relative('/data/docs', '/data/images/photo.jpg');
console.log("Relative path from /data/docs to /data/images/photo.jpg:", relativePath);
// Output: ../images/photo.jpg

// Real-world Example: Creating relative links
function createRelativeLink(currentFile: string, targetFile: string): string {
  const currentDir: string = path.dirname(currentFile);
  return path.relative(currentDir, targetFile);
}

const currentFile = '/docs/api/guides/getting-started.md';
const targetFile = '/docs/api/reference/classes.md';
const relativeLink = createRelativeLink(currentFile, targetFile);
console.log("Relative link from getting-started.md to classes.md:", relativeLink);
// Output: ../reference/classes.md
```

---

## 6. Constants

### 6.1 `path.sep: string`
Provides the platform-specific path segment separator.

```typescript
console.log("\n--- path.sep ---");
const separator: string = path.sep;
console.log("Platform path separator:", separator);
// Windows: "\", Linux/Mac: "/"

// Real-world Example: Splitting file paths correctly
const filePath: string = 'docs/api/user.txt';
const parts: string[] = filePath.split(path.sep);
console.log("Path parts:", parts);
// Output: ['docs', 'api', 'user.txt']
```

### 6.2 `path.delimiter: string`
Provides the platform-specific path delimiter.

```typescript
console.log("\n--- path.delimiter ---");
const delimiter: string = path.delimiter;
console.log("Platform path delimiter:", delimiter);
// Windows: ";", Linux/Mac: ":"

// Real-world Example: Reading PATH environment variable
const pathEnv = process.env.PATH || '';
const pathEntries: string[] = pathEnv.split(path.delimiter);
console.log("PATH environment variable entries:", pathEntries);
// Output: ['/usr/bin', '/usr/local/bin', ...] (example)
```

---

## 7. Platform-Specific Methods

### 7.1 `path.win32` and `path.posix`
Provide access to Windows or POSIX specific implementations.

```typescript
console.log("\n--- Platform-Specific Paths ---");
// Windows path handling
const winPath: string = path.win32.join('C:', 'Users', 'docs', 'file.txt');
console.log("Windows-style path:", winPath);
// Output: C:\Users\docs\file.txt

// POSIX path handling
const posixPath: string = path.posix.join('/home', 'user', 'docs', 'file.txt');
console.log("POSIX-style path:", posixPath);
// Output: /home/user/docs/file.txt

// Real-world Example: Cross-platform path handling
function ensurePosixPath(inputPath: string): string {
  if (path.sep === '\\') {
    return inputPath.replace(/\\/g, '/');
  }
  return inputPath;
}

const windowsStyle = 'src\\components\\Button.jsx';
const posixStyle = ensurePosixPath(windowsStyle);
console.log("Original Windows path:", windowsStyle);
console.log("Converted POSIX path:", posixStyle);
// Output: src/components/Button.jsx
```

### 7.2 `path.toNamespacedPath(path: string): string`
Converts to a namespace-prefixed path (mainly for Windows).

```typescript
console.log("\n--- path.toNamespacedPath() ---");
// Simple Example (Windows only)
if (process.platform === 'win32') {
  const nsPath: string = path.toNamespacedPath('C:\\Users\\file.txt');
  console.log("Namespaced path:", nsPath);
  // Output: \\?\C:\Users\file.txt
} else {
  console.log("This method is primarily useful on Windows systems");
}

// Real-world Example: Handling long paths on Windows
function handleLongPath(filePath: string): string {
  if (process.platform === 'win32' && filePath.length > 260) {
    return path.toNamespacedPath(filePath);
  }
  return filePath;
}

const longPath = 'C:\\very\\long\\path\\that\\exceeds\\windows\\limit\\file.txt';
const processedPath = handleLongPath(longPath);
console.log("Original path:", longPath);
console.log("Processed path:", processedPath);
```

---

## 8. Practical Examples with TypeScript

### 8.1 File Upload Handler with Type Interface
```typescript
console.log("\n--- Practical Example: File Upload Handler ---");
interface FileUploadInfo {
  originalName: string;
  savedName: string;
  fullPath: string;
  extension: string;
  uploadDir: string;
}

function handleFileUpload(originalName: string, uploadDir: string): FileUploadInfo {
  const extension: string = path.extname(originalName);
  const timestamp: number = Date.now();
  const uniqueName: string = `file_${timestamp}${extension}`;
  const fullPath: string = path.join(uploadDir, uniqueName);
  
  return {
    originalName,
    savedName: uniqueName,
    fullPath,
    extension,
    uploadDir
  };
}

const uploadInfo = handleFileUpload('document.pdf', '/uploads');
console.log("File upload information:", uploadInfo);
```

### 8.2 Configuration Loader with Type Safety
```typescript
console.log("\n--- Practical Example: Configuration Loader ---");
function loadConfig(configName: string): { configPath: string } {
  const configDir: string = process.env.NODE_ENV === 'production' 
    ? '/etc/app/config' 
    : path.join(__dirname, 'config');
  
  const configPath: string = path.join(configDir, `${configName}.json`);
  
  console.log(`Loading config from: ${configPath}`);
  return { configPath };
}

const dbConfig = loadConfig('database');
console.log("Database config path:", dbConfig.configPath);
```

### 8.3 Static File Server Helper with Error Handling
```typescript
console.log("\n--- Practical Example: Static File Server ---");
interface FileServeInfo {
  requested: string;
  served: string;
  exists: boolean;
}

function serveStaticFile(baseDir: string, requestedPath: string): FileServeInfo {
  const normalizedPath: string = path.normalize(requestedPath).replace(/^(\.\.(\/|\\|$))+/g, '');
  const absolutePath: string = path.resolve(path.join(baseDir, normalizedPath));
  
  if (!absolutePath.startsWith(baseDir)) {
    throw new Error('Invalid path');
  }
  
  return {
    requested: requestedPath,
    served: absolutePath,
    exists: true // In real code, check if file exists
  };
}

const fileInfo = serveStaticFile('/var/www', 'assets/images/logo.png');
console.log("File serving information:", fileInfo);
```

---

## 9. Summary Table

| Method/Constant | Description | Type Signature | Example |
|----------------|-------------|----------------|---------|
| `__dirname` | Current directory path | `string` | `console.log(__dirname)` |
| `__filename` | Current file path | `string` | `console.log(__filename)` |
| `path.join()` | Join path segments | `(...paths: string[]): string` | `path.join('a', 'b') → 'a/b'` |
| `path.resolve()` | Resolve to absolute path | `(...paths: string[]): string` | `path.resolve('a') → '/home/user/a'` |
| `path.normalize()` | Normalize path | `(path: string): string` | `path.normalize('a/../b') → 'b'` |
| `path.parse()` | Parse path into object | `(path: string): ParsedPath` | `path.parse('/a/b.txt') → ParsedPath` |
| `path.format()` | Format object to path | `(pathObject: FormatInputPathObject): string` | `path.format({dir: '/a', name: 'b', ext: '.txt'}) → '/a/b.txt'` |
| `path.basename()` | Get filename | `(path: string, ext?: string): string` | `path.basename('/a/b.txt') → 'b.txt'` |
| `path.dirname()` | Get directory name | `(path: string): string` | `path.dirname('/a/b.txt') → '/a'` |
| `path.extname()` | Get file extension | `(path: string): string` | `path.extname('b.txt') → '.txt'` |
| `path.isAbsolute()` | Check if absolute path | `(path: string): boolean` | `path.isAbsolute('/a') → true` |
| `path.relative()` | Get relative path | `(from: string, to: string): string` | `path.relative('/a', '/a/b') → 'b'` |
| `path.sep` | Path separator | `string` | `'a/b'.split(path.sep) → ['a', 'b']` |
| `path.delimiter` | Path delimiter | `string` | `PATH.split(path.delimiter) → string[]` |
| `path.win32` | Windows path methods | `object` | `path.win32.join('C:', 'file.txt')` |
| `path.posix` | POSIX path methods | `object` | `path.posix.join('/home', 'file.txt')` |
| `path.toNamespacedPath()` | Convert to namespaced path | `(path: string): string` | `path.toNamespacedPath('C:\\file.txt')` |

```typescript
console.log("\n=== End of Path Module Guide ===");
```

This comprehensive TypeScript guide includes a complete reference table at the beginning and proper console.log outputs for all examples, making it easy to understand and use the Node.js path module.