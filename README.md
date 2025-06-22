clangd does not complete usages of symbols defined in user headers (macOS)

1. When I start typing a symbol that is defined in a user header, clangd does not offer completion unless I have visited the .hpp header file. I also tried using .h headers to no avail.
2. After visiting the header file, completion works but header insertion still does not.

Completion and insertion for system headers works just fine.

Simple repository that has the issue: https://github.com/benjiwolff/cpp-hello-world
I just started using c++, there is no project where this does work for me.

Generated compile_command.json includes -I flag to `src` directory. I also tried moving the .hpp in a separate `include` directory.
```
{
  "directory": "/Users/benji/development/clangd-test/build",
  "command": "/opt/homebrew/opt/llvm/bin/clang++ -I/Users/benji/development/clangd-test/src  -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk -g -std=c++20 -arch arm64 @CMakeFiles/clang-test.dir/src/helloworld.cpp.o.modmap -o CMakeFiles/clang-test.dir/src/helloworld.cpp.o -c /Users/benji/development/clangd-test/src/helloworld.cpp",
  "file": "/Users/benji/development/clangd-test/src/helloworld.cpp",
  "output": "CMakeFiles/clang-test.dir/src/helloworld.cpp.o"
}
```

```
neovim clangd configuration
	require("lspconfig").clangd.setup({
		capabilities = require("cmp_nvim_lsp").default_capabilities(),
		cmd = {
			"/opt/homebrew/opt/llvm/bin/clangd",
			"--background-index",               -- also tried without this
			"--clang-tidy",                     -- also tried without this
			"--header-insertion=iwyu",          -- also tried without this
			"--header-insertion-decorators",    -- also tried without this
			"--completion-style=detailed",      -- also tried without this
			"--log=verbose",
		},
		filetypes = { "c", "cpp", "objc", "objcpp" },
		root_dir = require("lspconfig").util.root_pattern(".git"),
		single_file_support = true, -- also tried false
	})
```

// installed llvm via homebrew (not apple clang)
% clangd --version 
Homebrew clangd version 20.1.7
Features: mac+xpc
Platform: arm64-apple-darwin24.5.0

macOS 15.5 (24F74)
NVIM v0.12.0-dev-92+gfece489794
Build type: RelWithDebInfo
LuaJIT 2.1.1741730670
nvim-lspconfig: `7ad4a11cc5742774877c529fcfb2702f7caf75e4`

[Logs produced by these steps](./lsp.log)
1. open neovim
2. go to main.cpp
3. start typing "helloWorld" (no autocomplete happens)
4. open helloworld.hpp
5. back to main.cpp
6. start typeing "helloWorld" and use autocomplete
