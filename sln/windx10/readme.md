# ImGui Windows DirectX10

注：参考了 examples 下 build_win32.bat

导入文件

$(SolutionDir)..\..\lib\imgui
$(SolutionDir)..\..\lib\imgui\backends

链接 输入

d3d10.lib
d3dcompiler.lib

加入源文件

..\..\backends\imgui_impl_win32.cpp ..\..\backends\imgui_impl_dx10.cpp ..\..\imgui*.cpp