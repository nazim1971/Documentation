# 📘 Node.js Path Module - Complete Guide

The `path` module in Node.js provides utilities for working with file and directory paths. It helps you handle and transform file paths in a way that works across different operating systems.

## Installation & Import

No installation needed - the path module is part of Node.js core.

```javascript
// Using ES6 modules (Node.js 14+)
import path from 'path';

// Using CommonJS
const path = require('path');
```

## Constants

### `path.sep`
Platform-specific path segment separator.

```javascript
console.log('Separator:', path.sep);
// Windows: "\"
// Linux/Mac: "/"

// Real-life example: Splitting a file path
const filePath = 'docs/api/user.txt';
const parts = filePath.split(path.sep);
console.log('Path parts:', parts); // ['docs', 'api', 'user.txt']
```

### `path.delimiter`
Platform-specific path delimiter (used in environment variables like PATH).

```javascript
console.log('Delimiter:', path.delimiter);
// Windows: ";"
// Linux/Mac: ":"

// Real-life example: Reading PATH environment variable
const pathEntries = process.env.PATH.split(path.delimiter);
console.log('PATH entries:', pathEntries);
```

### `__dirname` and `__filename`
Global variables available in each module.

```javascript
console.log('Current directory:', __dirname);
console.log('Current file:', __filename);

// Real-life example: Creating paths relative to current file
const configPath = path.join(__dirname, 'config', 'app.json');
console.log('Config path:', configPath);
```

## Path Composition Methods

### `path.join([...paths])`
Joins path segments and normalizes the result.

```javascript
// Demo example
const fullPath = path.join('users', 'docs', 'file.txt');
console.log('Joined path:', fullPath);
// Output: users/docs/file.txt (Linux/Mac) or users\docs\file.txt (Windows)

// Real-life example: Building API endpoint paths
const apiVersion = 'v1';
const endpoint = 'users';
const resource = 'profile';
const apiPath = path.join('/api', apiVersion, endpoint, resource);
console.log('API path:', apiPath); // /api/v1/users/profile
```

### `path.resolve([...paths])`
Resolves path segments into an absolute path.

```javascript
// Demo example
const absolutePath = path.resolve('src', 'app.js');
console.log('Absolute path:', absolutePath);
// Output: /home/user/project/src/app.js (example)

// Real-life example: Resolving project root paths
const projectRoot = path.resolve(__dirname, '..');
const assetsPath = path.resolve(projectRoot, 'assets', 'images');
console.log('Assets directory:', assetsPath);
```

### `path.normalize(path)`
Normalizes a path by resolving '..' and '.' segments.

```javascript
// Demo example
const messyPath = 'docs/../src/./app//utils';
const cleanPath = path.normalize(messyPath);
console.log('Normalized path:', cleanPath); // src/app/utils

// Real-life example: Cleaning user-provided file paths
function safePath(userInput) {
  const normalized = path.normalize(userInput);
  // Prevent directory traversal attacks
  if (normalized.startsWith('..')) {
    throw new Error('Invalid path');
  }
  return normalized;
}

console.log('Safe path:', safePath('files/../docs/./report.txt')); // docs/report.txt
```

## Path Decomposition Methods

### `path.parse(path)`
Parses a path into its components.

```javascript
// Demo example
const parsed = path.parse('/home/user/docs/report.pdf');
console.log('Parsed path:', parsed);
/* Output:
{
  root: '/',
  dir: '/home/user/docs',
  base: 'report.pdf',
  ext: '.pdf',
  name: 'report'
}
*/

// Real-life example: File upload processing
function processUpload(filePath) {
  const { name, ext, dir } = path.parse(filePath);
  
  return {
    originalName: filePath,
    filename: `${name}-${Date.now()}${ext}`,
    uploadDir: path.join(dir, 'uploads')
  };
}

const uploadInfo = processUpload('/tmp/docs/report.pdf');
console.log('Upload info:', uploadInfo);
```

### `path.format(pathObject)`
Formats a path object into a path string.

```javascript
// Demo example
const pathObj = {
  dir: '/home/user/docs',
  name: 'report',
  ext: '.pdf'
};
const formattedPath = path.format(pathObj);
console.log('Formatted path:', formattedPath); // /home/user/docs/report.pdf

// Real-life example: Generating download file paths
function createDownloadPath(userId, filename) {
  const { name, ext } = path.parse(filename);
  
  return path.format({
    dir: path.join('/downloads', userId),
    name: `${name}-${Date.now()}`,
    ext: ext
  });
}

const downloadPath = createDownloadPath('user123', 'document.docx');
console.log('Download path:', downloadPath);
```

### `path.basename(path[, ext])`
Gets the last portion of a path.

```javascript
// Demo example
console.log('Basename:', path.basename('/docs/api/user.json')); // user.json
console.log('Without extension:', path.basename('/docs/api/user.json', '.json')); // user

// Real-life example: File processing based on extension
function processFile(filePath) {
  const extension = path.extname(filePath);
  const filename = path.basename(filePath, extension);
  
  if (extension === '.json') {
    console.log(`Processing JSON file: ${filename}`);
  } else if (extension === '.csv') {
    console.log(`Processing CSV file: ${filename}`);
  }
}

processFile('/data/users.json'); // Processing JSON file: users
```

### `path.dirname(path)`
Gets the directory name of a path.

```javascript
// Demo example
console.log('Directory name:', path.dirname('/docs/api/user.json')); // /docs/api

// Real-life example: Creating backup directories
function createBackup(filePath) {
  const dir = path.dirname(filePath);
  const backupDir = path.join(dir, 'backups');
  const filename = path.basename(filePath);
  
  console.log(`Creating backup of ${filename} in ${backupDir}`);
  // fs.mkdirSync(backupDir, { recursive: true });
  // fs.copyFileSync(filePath, path.join(backupDir, filename));
}

createBackup('/data/config.json');
```

### `path.extname(path)`
Gets the extension of a path.

```javascript
// Demo example
console.log('Extension:', path.extname('document.pdf')); // .pdf
console.log('Extension:', path.extname('archive.tar.gz')); // .gz

// Real-life example: File type validation
function isValidFileType(filePath, allowedTypes) {
  const extension = path.extname(filePath).toLowerCase();
  return allowedTypes.includes(extension);
}

const allowed = ['.jpg', '.jpeg', '.png', '.gif'];
console.log('Valid image?', isValidFileType('photo.jpg', allowed)); // true
console.log('Valid image?', isValidFileType('document.pdf', allowed)); // false
```

## Path Information Methods

### `path.isAbsolute(path)`
Checks if a path is absolute.

```javascript
// Demo example
console.log('Is absolute?', path.isAbsolute('/home/user/file.txt')); // true
console.log('Is absolute?', path.isAbsolute('docs/file.txt')); // false

// Real-life example: Path validation in web applications
function validateFilePath(userInput) {
  if (path.isAbsolute(userInput)) {
    throw new Error('Absolute paths are not allowed');
  }
  
  if (userInput.includes('..')) {
    throw new Error('Parent directory references are not allowed');
  }
  
  return path.normalize(userInput);
}

try {
  const safePath = validateFilePath('docs/reports/summary.txt');
  console.log('Valid path:', safePath);
} catch (error) {
  console.error('Error:', error.message);
}
```

### `path.relative(from, to)`
Returns the relative path from one directory to another.

```javascript
// Demo example
const relativePath = path.relative('/data/docs', '/data/images/photo.jpg');
console.log('Relative path:', relativePath); // ../images/photo.jpg

// Real-life example: Creating relative links in documentation
function createRelativeLink(currentFile, targetFile) {
  const currentDir = path.dirname(currentFile);
  return path.relative(currentDir, targetFile);
}

const currentFile = '/docs/api/guides/getting-started.md';
const targetFile = '/docs/api/reference/classes.md';
const relativeLink = createRelativeLink(currentFile, targetFile);
console.log('Relative link:', relativeLink); // ../reference/classes.md
```

## Platform-Specific Methods

### `path.win32` and `path.posix`
Access Windows or POSIX specific implementations.

```javascript
// Demo example
const winPath = path.win32.join('C:', 'Users', 'docs', 'file.txt');
console.log('Windows path:', winPath); // C:\Users\docs\file.txt

const posixPath = path.posix.join('/home', 'user', 'docs', 'file.txt');
console.log('POSIX path:', posixPath); // /home/user/docs/file.txt

// Real-life example: Cross-platform path handling in build tools
function ensurePosixPath(inputPath) {
  if (path.sep === '\\') {
    // On Windows, convert to POSIX style
    return inputPath.replace(/\\/g, '/');
  }
  return inputPath;
}

const windowsStyle = 'src\\components\\Button.jsx';
const posixStyle = ensurePosixPath(windowsStyle);
console.log('POSIX style path:', posixStyle); // src/components/Button.jsx
```

### `path.toNamespacedPath(path)`
Converts to a namespace-prefixed path (mainly for Windows).

```javascript
// Demo example (mainly useful on Windows)
const nsPath = path.toNamespacedPath('C:\\Users\\file.txt');
console.log('Namespaced path:', nsPath); // \\?\C:\Users\file.txt

// Real-life example: Handling long paths on Windows
function handleLongPath(filePath) {
  if (process.platform === 'win32' && filePath.length > 260) {
    return path.toNamespacedPath(filePath);
  }
  return filePath;
}

const longPath = 'C:\\very\\long\\path\\that\\exceeds\\windows\\limit\\file.txt';
const processedPath = handleLongPath(longPath);
console.log('Processed path:', processedPath);
```

## Practical Examples

### Example 1: File Upload Handler

```javascript
function handleFileUpload(originalName, uploadDir) {
  // Get file extension
  const extension = path.extname(originalName);
  
  // Create unique filename
  const timestamp = Date.now();
  const uniqueName = `file_${timestamp}${extension}`;
  
  // Create full path
  const fullPath = path.join(uploadDir, uniqueName);
  
  return {
    originalName,
    savedName: uniqueName,
    fullPath,
    extension,
    uploadDir
  };
}

const uploadInfo = handleFileUpload('document.pdf', '/uploads');
console.log('File upload info:', uploadInfo);
```

### Example 2: Configuration Loader

```javascript
function loadConfig(configName) {
  // Determine config path based on environment
  const configDir = process.env.NODE_ENV === 'production' 
    ? '/etc/app/config' 
    : path.join(__dirname, 'config');
  
  const configPath = path.join(configDir, `${configName}.json`);
  
  console.log(`Loading config from: ${configPath}`);
  // const configData = fs.readFileSync(configPath, 'utf8');
  // return JSON.parse(configData);
  
  return { configPath };
}

const dbConfig = loadConfig('database');
console.log('Database config path:', dbConfig.configPath);
```

### Example 3: Static File Server Helper

```javascript
function serveStaticFile(baseDir, requestedPath) {
  // Prevent directory traversal attacks
  const normalizedPath = path.normalize(requestedPath).replace(/^(\.\.(\/|\\|$))+/g, '');
  
  // Resolve to absolute path
  const absolutePath = path.resolve(path.join(baseDir, normalizedPath));
  
  // Ensure the path is within the base directory
  if (!absolutePath.startsWith(baseDir)) {
    throw new Error('Invalid path');
  }
  
  return {
    requested: requestedPath,
    served: absolutePath,
    exists: true // fs.existsSync(absolutePath)
  };
}

const fileInfo = serveStaticFile('/var/www', 'assets/images/logo.png');
console.log('File serving info:', fileInfo);
```

## Summary Table

| Method | Description | Example |
|--------|-------------|---------|
| `path.join()` | Join path segments | `path.join('a', 'b')` → `a/b` |
| `path.resolve()` | Resolve to absolute path | `path.resolve('a')` → `/home/user/a` |
| `path.normalize()` | Normalize path | `path.normalize('a/../b')` → `b` |
| `path.parse()` | Parse path into object | `path.parse('/a/b.txt')` → Object |
| `path.basename()` | Get filename | `path.basename('/a/b.txt')` → `b.txt` |
| `path.dirname()` | Get directory name | `path.dirname('/a/b.txt')` → `/a` |
| `path.extname()` | Get file extension | `path.extname('b.txt')` → `.txt` |
| `path.isAbsolute()` | Check if absolute | `path.isAbsolute('/a')` → `true` |
| `path.relative()` | Get relative path | `path.relative('/a', '/a/b')` → `b` |

## Browser Compatibility Note

The path module is a Node.js core module and is not available in browsers. For browser-based path manipulation, consider using the `URL` API or path manipulation libraries.

This documentation follows the practical, example-focused approach similar to W3Schools, making it easy to understand and implement in real projects.