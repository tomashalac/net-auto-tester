# .Net auto tester
Compile and Test .NET projects in (Windows, Mac and Linux) without having to configure anything, Using github actions.

# Installation
Go to the Actions tab, and click on **"Set up a workflow yourself"** and paste this configuration:
```yml
name: Build And Test .NET Core Projects

on: [push, pull_request]

jobs:
  build:

    runs-on: ${{ matrix.os }}
    
    strategy:
      matrix:
        os: [macOS-latest, ubuntu-latest, windows-latest]

    steps:
    - uses: actions/checkout@v2
    - name: Setup .NET Core
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: 2.2.108

    # Linux
    - name: Build with dotnet on Linux
      run: dotnet build ./**/*.sln --configuration Release
      if: (matrix.os != 'windows-latest')
    # Linux
    - name: Test with dotnet on Linux
      run: dotnet test ./**/*.sln
      if: (matrix.os != 'windows-latest')

    # Build And Test Windows
    - name: Build And Test on Windows
      run: |
        dir /S /b "%GITHUB_WORKSPACE%\*.sln" > result.txt
        set /p SOLUTION_FILE=<result.txt
        dotnet build %SOLUTION_FILE% --configuration Release
        dotnet test %SOLUTION_FILE%
      if: (matrix.os == 'windows-latest')
      shell: cmd
```
And then click on *"Start commit"* and do the commit and voila.