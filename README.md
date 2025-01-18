# Clarion Builder MCP Server

An MCP server providing Clarion development and build capabilities. This server enables automated Clarion IDE operations, project compilation, and build management using ClarionCL and MSBuild.

## Features

- ClarionCL command execution
- Project generation and compilation
- Template management
- Dictionary import/export
- TXA file handling
- MSBuild integration
- Comprehensive build configuration

## Tools

### ClarionCL Operations

#### clarion_cl
Execute ClarionCL commands for IDE tasks.
- **action**: ClarionCL action to perform (required)
  - Values:
    - `generate`: Generate project files
    - `import-txa`: Import TXA files
    - `export-txa`: Export TXA files
    - `export-dict`: Export dictionary
    - `import-dict`: Import dictionary
    - `register-template`: Register a template
    - `unregister-template`: Unregister a template
    - `list-templates`: List registered templates
    - `register-driver`: Register a driver
- **filePath**: Path to APP/SLN/TXA/DCT file
- **secondaryPath**: Secondary path for import/export operations
- **version**: Clarion version to use (e.g., "Clarion 11.0 Enterprise Edition")
- **conditionalGeneration**: Enable conditional generation for generate action
- **debugGeneration**: Enable debug generation for generate action
- **useWindowsRedirection**: Use Windows redirection file (/win switch) (required)
- **templateClass**: Template class name for template operations
- **redirectionFile**: Redirection file to use (e.g., "Clarion110.red") (required)

### Solution Building

#### compile_solution
Compile a Clarion solution using MSBuild.
- **solutionPath**: Full path to the .sln file (required)
- **projectName**: Name of specific project to compile (e.g., MyApp.app)
- **targetName**: Name of specific target/exe to build (e.g., MyApp.exe)
- **configuration**: Build configuration
  - Values: 'Debug' or 'Release'
- **platform**: Target platform
  - Values: 'Win32' or 'x64'
- **clarionBinPath**: Path to Clarion binaries
- **logFile**: Path for build log file
- **additionalArgs**: Additional MSBuild arguments

Build Options:
- **checkIndex**: Generate array index check code
- **checkStack**: Generate stack access check code
- **lineNumbers**: Add line numbers to MAP file
- **generateMap**: Generate MAP file
- **stackSize**: Stack size
- **vid**: Debug support level
  - Values: 'full', 'min', 'off'
- **model**: Memory model
  - Values: 'Dll', 'Lib', 'CustomDll'
- **copyCoreFiles**: Copy core DLLs
- **defines**: Semicolon-separated list of defines

## Requirements

- Clarion 11.0 or higher
- Microsoft .NET Framework 4.0 or higher
- Visual Studio build tools
- Windows SDK

## Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/clarion-builder-mcp-server.git
cd clarion-builder-mcp-server
```

2. Install dependencies:
```bash
npm install
```

3. Build the project:
```bash
npm run build
```

## Configuration

Add the server to your MCP settings file:

```json
{
  "mcpServers": {
    "clarion-builder": {
      "command": "node",
      "args": ["path/to/clarion-builder-mcp-server/dist/index.js"],
      "env": {}
    }
  }
}
```

## Usage Examples

### Generate Project

```typescript
await mcp.use("clarion-builder", "clarion_cl", {
  action: "generate",
  filePath: "./src/MyApp.app",
  version: "Clarion 11.0 Enterprise Edition",
  conditionalGeneration: true,
  useWindowsRedirection: true,
  redirectionFile: "Clarion110.red"
});
```

### Import/Export TXA

```typescript
// Export TXA
await mcp.use("clarion-builder", "clarion_cl", {
  action: "export-txa",
  filePath: "./src/MyApp.app",
  secondaryPath: "./backup/MyApp.txa",
  useWindowsRedirection: true,
  redirectionFile: "Clarion110.red"
});

// Import TXA
await mcp.use("clarion-builder", "clarion_cl", {
  action: "import-txa",
  filePath: "./src/MyApp.app",
  secondaryPath: "./backup/MyApp.txa",
  useWindowsRedirection: true,
  redirectionFile: "Clarion110.red"
});
```

### Compile Solution

```typescript
await mcp.use("clarion-builder", "compile_solution", {
  solutionPath: "./MyApp.sln",
  configuration: "Release",
  platform: "Win32",
  generateMap: true,
  vid: "full",
  model: "Dll",
  copyCoreFiles: true
});
```

## Development

1. Make changes to the source code
2. Run tests:
```bash
npm test
```
3. Build the project:
```bash
npm run build
```

## Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

MIT