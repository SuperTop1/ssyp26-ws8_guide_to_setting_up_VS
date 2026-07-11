# Guide to setting up VS Code for the SFML library

1. install [WinLibs UCRT 14.2.0](https://github.com/brechtsanders/winlibs_mingw/releases/download/14.2.0posix-19.1.1-12.0.0-ucrt-r2/winlibs-i686-posix-dwarf-gcc-14.2.0-mingw-w64ucrt-12.0.0-r2.7z)
2. unzip WinLibs UCRT 14.2.0
3. install [SFML 3.1.0](https://www.sfml-dev.org/files/SFML-3.1.0-windows-gcc-14.2.0-mingw-32-bit.zip)
4. unzip SFML 3.1.0
5. Open VS Code and press <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd>
6. Type and select `Tasks: Configure Task`
7. Configure the `tasks.json` file by replacing its content with the following code. 

> **Important:** Make sure to replace `path_to_your_WinLibs` and `path_to_your_SFML` with your actual local absolute paths. Use double backslashes `\\` in paths.
```json
{
    "tasks": [
        {
            "type": "cppbuild",
            "label": "C/C++: g++.exe active file build",
            "command": "path_to_your_WinLibs\\bin\\g++.exe",
            "args": [
                "-fdiagnostics-color=always",
                "-g",
                "-DSFML_STATIC",
                "-Ipath_to_your_SFML\\include",
                "${file}",
                "-Lpath_to_your_SFML\\lib",
                "-lsfml-graphics-s",
                "-lsfml-window-s",
                "-lsfml-system-s",
                "-lopengl32",
                "-lfreetype",
                "-lwinmm",
                "-lgdi32",
                "-static-libgcc",
                "-static-libstdc++",
                "-static",
                "-lwinpthread",
                "-o",
                "${fileDirname}\\${fileBasenameNoExtension}.exe"
            ],
            "options": {
                "cwd": "${fileDirname}"
            },
            "problemMatcher": [
                "$gcc"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "detail": "The task was created by the debugger."
        }
    ],
    "version": "2.0.0"
}
```

8. To remove the red squiggly error lines, press <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd> again.
9. Type and select `C/C++: Edit Configurations (JSON)`.
10. Edit the `c_cpp_properties.json` file.

```json
{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
                "${workspaceFolder}/**",
                "path_to_your_SFML\\include"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE",
                "SFML_STATIC"
            ],
            "windowsSdkVersion": "10.0.19041.0",
            "compilerPath": "path_to_your_WinLibs\\bin\\g++.exe",
            "cStandard": "c17",
            "cppStandard": "c++17",
            "intelliSenseMode": "windows-gcc-x64"
        }
    ],
    "version": 4
}
```
---
### Code for example working SFML:
```cpp
#include <SFML/Graphics.hpp>

int main(){
    sf::RenderWindow window(sf::VideoMode({300, 300}), "Title");

    sf::CircleShape circle(10.f);
    circle.setPosition({100.f, 100.f});
    circle.setFillColor(sf::Color(255, 0, 0));

    while(window.isOpen()){
        while(auto event = window.pollEvent()){
            if(event->is<sf::Event::Closed>()){
                window.close();
            }
        }
        window.clear(sf::Color(255, 255, 255));
        window.draw(circle);
        window.display();
    }
    return 0;
}
```
