## 书途（BOOKPATH）
### 一、简介：
书途(*BookPath*)系统是由上海交通大学AIUI课程项目组五名成员为图书馆开发的一款创新智能程序，旨在为用户提供精准的图书推荐服务，并规划最优的图书馆内导航路线。本项目基于[Gradio](https://github.com/gradio-app/gradio)以及[Langchain-Chatchat](https://github.com/chatchat-space/Langchain-Chatchat)进行开发。用户只需输入需求，系统将根据算法推荐相关书籍，并生成一条从当前位置到目标书籍所在位置的最优路径。无论是学生、研究人员，还是书籍爱好者，该系统都能大幅提升寻找和借阅图书的效率，节省时间，提升体验。

主要功能包括：
1. **智能图书推荐**：根据用户输入的需求，结合大数据和人工智能算法，推荐相关书籍。
2. **最优路线规划**：为用户规划从当前位置到目标书籍所在位置的最优路线。
3. **实时导航**（可能在未来添加）：在图书馆内提供实时导航服务，确保用户能够快速找到所需图书。
<br><br>
### 二、项目结构与简介

```
BookPath/
│
├── configs # 模型及模型的一些设置
├── server # 模型服务器
├── knowledge_base # 知识库
├── model # 模型文件（由于模型文件过大，我们并未上传）
├── Library # 关于图书馆的一些配置文件
    ├── library.jpg # 图书馆的彩色地图
    ├── library_gray.jpg # 图书馆的灰度地图，用于导航
    ├── books.json # 书的配置文件
    └── location.json # 书架位置的配置文件
├── README.md # 项目简介文件
├── start_up.py # Longchain_Chatchat启动脚本
├── setup_books.py # 图书配置脚本
├── setup_location.py # 位置配置脚本
└── ui_gradio.py # gradio前端启动脚本
```


**项目模块说明：**

1. **configs/**: 存放模型及其相关设置文件，用于配置和管理机器学习模型。
2. **server/**: 包含模型服务器的相关代码，负责处理模型的加载和推理请求。
3. **knowledge_base/**: 存储知识库，包含图书推荐算法所需的数据和信息。
4. **Library/**: 存放与图书馆相关的配置文件，包括图书馆地图、书籍信息和书架位置信息。
   - **library.jpg**: 图书馆的彩色地图，用于用户界面展示。
   - **library_gray.jpg**: 图书馆的灰度地图，用于导航算法。
   - **books.json**: 包含图书的详细信息配置文件。
   - **location.json**: 书架位置的配置文件，描述图书馆内各书架的位置。
5. **README.md**: 项目简介文件，包含项目的整体介绍和使用说明。
6. **start_up.py**: Longchain_Chatchat的启动脚本，用于初始化和启动整个系统。
7. **setup_books.py**: 图书配置脚本，用于设置和更新图书信息。
8. **setup_location.py**: 位置配置脚本，用于设置和更新书架位置信息。
9. **ui_gradio.py**: gradio前端启动脚本，用于启动图形用户界面，提供用户交互入口。
<br><br>
### 三、配置步骤：

按照以下步骤设置 BookPath 项目：

#### 第一步：安装依赖
首先，安装必要的依赖：
```
pip install -r requirements.txt
```

#### 第二步：修改图书馆配置文件
根据具体的图书馆设置，更新 `Library/` 目录中的文件。

- **`location.json`** 的格式如下：
  ```json
  {
    "0": [602, 210]
  }
  ```
  其中，`"0"` 是位置编号，`[602, 210]` 是图像中的坐标。

- **`books.json`** 的格式如下：
  ```json
  {
    "0": ["红楼梦"]
  }
  ```
  其中，`"0"` 是位置编号，`["红楼梦"]` 是该位置的图书列表。

你可以借助 `setup_books.py` 和 `setup_location.py` 脚本完成这些配置文件的设置。

#### 第三步：处理图书馆地图
使用 `setup_map.py` 将 `Library/library.jpg` 转换为二值化后的地图。注意，自适应二值化方法只会给出一个粗糙的结果，精细调整可能需要其他的计算机视觉方法或使用 Photoshop 等软件进行手动编辑。

#### 第四步：模型和 API 配置
参考 Langchain-Chatchat 的官方文档了解如何使用模型和 API。如果需要在本地运行大型模型，请将它们放置在 `/model` 目录下，并修改 `model_config.py` 中的相关设置。
<br><br>

以下是我们使用的模型的链接：

### 四、运行方法

**！！重要！！**

强烈建议你在云端部署本项目（具有较大显存的GPU和足够的磁盘空间）

#### 第一步：启动所有 API
运行以下命令启动所有必要的服务：
```
python3 startup.py --all-api
#Windows命令为python startup.py --all-api
```

#### 第二步：启动前端
在所有服务启动完成后，打开一个新的终端，运行：
```
python3 ui_gradio.py
#Windows命令为 python ui_gradio.py
```
你应该会在输出中看到：
```
Running on URL: 127.0.0.1:9001
```
在浏览器中打开该 URL 即可访问应用。

注：你也可以尝试运行：
```
python3 startup_neo.py --all-api --webui
```

<br><br>
### 五、注意事项

1. **兼容性问题：**
   如果遇到 `gradio` 和 `fastapi` 的冲突问题，请尝试安装特定版本的 `gradio`：
   ```
   pip install gradio==3.33.0
   ```

2. **代理环境：**
   如果本地具有代理环境，需要关闭代理才能正常运行应用。

3. **Python版本**

   Python >= 3.9 (推荐 3.10 和 3.11)

4. **端口**

   确保运行程序时端口没有被占用

   你可以在`configs/server_config.py`中修改服务器的端口设置

   运行

   ```
   python3 ui_gradio.py --gradio_port=<your port>
   ```

   以指定gradio服务的端口

   如果你不能重启本项目，尝试运行

   ```
   ./shutdown_all.sh
   ```

   以杀死之前启动的进程  

