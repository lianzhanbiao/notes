open3d提供了键盘交互的接口。
## 1、c++如何实现与键盘通信
参考[https://www.runoob.com/w3cnote/c-get-keycode.html](https://www.runoob.com/w3cnote/c-get-keycode.html)
### 1.1、Windows 系统下的 vs 中可以使用 _kbhit() 函数来获取键盘事件，使用时需要加入 conio.h 头文件，例：
```cpp
#include <conio.h>
#include <iostream>
 
using namespace std;
 
int main()
{
    int ch;
    while (1){
        if (_kbhit()){//如果有按键按下，则_kbhit()函数返回真
            ch = _getch();//使用_getch()函数获取按下的键值
            cout << ch;
            if (ch == 27){ break; }//当按下ESC时循环，ESC键的键值时27.
        }
    }
    system("pause");
}
```
### 1.2、键盘 Key Code 对照表
| **字母和数字键的键码值(keyCode)** |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 按键 | 键码 | 按键 | 键码 | 按键 | 键码 | 按键 | 键码 |
| A | 65 | J | 74 | S | 83 | 1 | 49 |
| B | 66 | K | 75 | T | 84 | 2 | 50 |
| C | 67 | L | 76 | U | 85 | 3 | 51 |
| D | 68 | M | 77 | V | 86 | 4 | 52 |
| E | 69 | N | 78 | W | 87 | 5 | 53 |
| F | 70 | O | 79 | X | 88 | 6 | 54 |
| G | 71 | P | 80 | Y | 89 | 7 | 55 |
| H | 72 | Q | 81 | Z | 90 | 8 | 56 |
| I | 73 | R | 82 | 0 | 48 | 9 | 57 |

| **数字键盘上的键的键码值(keyCode)** |  |  |  | **功能键键码值(keyCode)** |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 按键 | 键码 | 按键 | 键码 | 按键 | 键码 | 按键 | 键码 |
| 0 | 96 | 8 | 104 | F1 | 112 | F7 | 118 |
| 1 | 97 | 9 | 105 | F2 | 113 | F8 | 119 |
| 2 | 98 | * | 106 | F3 | 114 | F9 | 120 |
| 3 | 99 | + | 107 | F4 | 115 | F10 | 121 |
| 4 | 100 | Enter | 108 | F5 | 116 | F11 | 122 |
| 5 | 101 | - | 109 | F6 | 117 | F12 | 123 |
| 6 | 102 | . | 110 |   |   |   |   |
| 7 | 103 | / | 111 |   |   |   |   |

| **控制键键码值(keyCode)** |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 按键 | 键码 | 按键 | 键码 | 按键 | 键码 | 按键 | 键码 |
| BackSpace | 8 | Esc | 27 | Right Arrow | 39 | -_ | 189 |
| Tab | 9 | Spacebar | 32 | Dw Arrow | 40 | .> | 190 |
| Clear | 12 | Page Up | 33 | Insert | 45 | /? | 191 |
| Enter | 13 | Page Down | 34 | Delete | 46 | `~ | 192 |
| Shift | 16 | End | 35 | Num Lock | 144 | [{ | 219 |
| Control | 17 | Home | 36 | ;: | 186 | /&#124; | 220 |
| Alt | 18 | Left Arrow | 37 | =+ | 187 | ]} | 221 |
| Cape Lock | 20 | Up Arrow | 38 | ,< | 188 | '" | 222 |

| **多媒体键码值(keyCode)** |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 按键 | 键码 | 按键 | 键码 | 按键 | 键码 | 按键 | 键码 |
| 音量加 | 175 |   |   |   |   |   |   |
| 音量减 | 174 |   |   |   |   |   |   |
| 停止 | 179 |   |   |   |   |   |   |
| 静音 | 173 |   |   |   |   |   |   |
| 浏览器 | 172 |   |   |   |   |   |   |
| 邮件 | 180 |   |   |   |   |   |   |
| 搜索 | 170 |   |   |   |   |  |  |
| 收藏 | 171 |   |   |   |   |   |   |

## 2、open3d的泊送重建
open3d的播送重建参考的官方实现，可以获取density属性，提供了裁剪的方法。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2473100/1662184227498-699034cb-9aa1-4473-a895-7639ab627f7c.png#clientId=u902c3c10-6600-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=269&id=u08cd84bf&margin=%5Bobject%20Object%5D&name=image.png&originHeight=269&originWidth=1260&originalType=binary&ratio=1&rotation=0&showTitle=false&size=34818&status=done&style=none&taskId=u72f9b97c-117e-4d25-85f7-47d84524392&title=&width=1260)
函数open3d:geometry::TriangleMesh::CreateFromPointCloudPoisson
接收带法向量的点云、八叉树深度等参数，返回mesh和densities数组。
参考使用方法：
```cpp
auto mesh_with_density = open3d::geometry::TriangleMesh::CreateFromPointCloudPoisson(*cloudptr, 12, 0, 1, false, 16);
auto mesh = std::get<0>(mesh_with_density);
auto densities = std::get<1>(mesh_with_density);//density数组的下标就是对应顶点vertex的下标
```
如果要裁剪掉某一density范围的顶点及其对应的三角网格
```cpp
mesh->RemoveVerticesByIndex(verticesToRemove);//verticesToRemove是要删除的顶点下标
```
## 3、open3d提供的键盘事件接口
参考[http://www.open3d.org/docs/release/cpp_api/classopen3d_1_1visualization_1_1_visualizer_with_key_callback.html](http://www.open3d.org/docs/release/cpp_api/classopen3d_1_1visualization_1_1_visualizer_with_key_callback.html)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2473100/1662183864845-1dfd0d1c-39dd-42ea-a82f-b3baf3c42251.png#clientId=u902c3c10-6600-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=379&id=u33596c68&margin=%5Bobject%20Object%5D&name=image.png&originHeight=379&originWidth=918&originalType=binary&ratio=1&rotation=0&showTitle=false&size=44045&status=done&style=none&taskId=u3286aa1a-e991-43ba-a5b7-cce0b7b82d7&title=&width=918)
如图所示，该类提供了接收键盘事的回调函数作为参数的成员函数。
这里实现两个成员函数用于接收键盘事件并裁剪mesh。
```cpp
bool key_up_callback(open3d::visualization::Visualizer* visualizer)
{
	std::cout << "key_up_callback\n";
	open3d::geometry::TriangleMesh m;
	m += *mesh;
	auto mptr = std::make_shared<open3d::geometry::TriangleMesh>(m);
	remove_percent -= 0.05;
	getVerticesToRemove(min_density + remove_percent * delt, densities, verticesToRemove);
	mptr->RemoveVerticesByIndex(verticesToRemove);
	showmesh(*visualizer, mptr);
	return true;
}

bool key_down_callback(open3d::visualization::Visualizer* visualizer)
{
	std::cout << "key_down_callback\n";
	open3d::geometry::TriangleMesh m;
	m += *mesh;
	auto mptr = std::make_shared<open3d::geometry::TriangleMesh>(m);
	remove_percent += 0.05;
	getVerticesToRemove(min_density + remove_percent * delt, densities, verticesToRemove);
	mptr->RemoveVerticesByIndex(verticesToRemove);
	showmesh(*visualizer, mptr);
	return true;
}
```
注册
```cpp
open3d::visualization::VisualizerWithKeyCallback visualizer;;
visualizer.CreateVisualizerWindow("使用A或D键，对mesh进行裁剪", 1500, 1000);
visualizer.RegisterKeyCallback(65, key_up_callback);
visualizer.RegisterKeyCallback(97, key_up_callback);
visualizer.RegisterKeyCallback(68, key_down_callback);
visualizer.RegisterKeyCallback(100, key_down_callback);
```
最终效果：
![](./0a32132d27754ec49114802eb4eb32c7.gif)

 
