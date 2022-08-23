## OSG模型文件存储与读取
#### 读取文件功能

	void OsgWidget::loadOsg(QString osgFilePath)
	{
	    _pViewer->setSceneData(osgDB::readNodeFile(osgFilePath.toStdString()));
	}

#### 存入文件功能

	bool OsgWidget::saveNode(osg::ref_ptr<osg::Node> pNode, QString fileName)
	{
	    return osgDB::writeNodeFile(*pNode.get(), fileName.toStdString());
	}

#### OSG读取与保存详解
- osgDB/readFile：osg所有读取磁盘文件函数都在此头文件中
	- osg打开文件是依据后缀名打开的，使用不同的打开函数返回不同的数据类型指针，文件名参数中可以包含绝对路径或者相对路径。
		- 如果用户指定了绝对路径， OSG 将到指定的位置去搜索文件。
		- 如果用户制定了相对路劲或者不包含路径，OSG将使用osgDB的数据文件路径表来搜索文件，用户可以通过设置环境变量 OSG_FILE_PATH 来设置这个目录列表。
	- 要添加指定的数据目录到数据文件路径表，可以使用`osgDB::Registry::getDataFilePathList()` 函数。osgDB::Registry是一个单态类（singleton），因此要调用这个函数的话，需要使用单态类的实例。函数会返回一个 osgDB::FilePathList 的指针，也就是一个简单的 std::deque<std::string>。假如要从一个字符串 newpath 中添加目录到列表的话，可以使用下面的代码：
	
			osgDB::Registry::instance()->getDataFilePathList().push_back(newpath);
	- 如果因为某些原因， OSG 不能读取文件，那么这两个函数都会返回 NULL指针。要查看文件读取失败的原因的话，可以设置 OSG_NOTIFY_LEVEL 环境变量为一个较高的值（例如 WARN），然后再次尝试读取文件，并检查程序控制台输出的警告或者错误信息。
	<table border="1" cellspacing="0"><tbody><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;"><strong>序号</strong></p> </td><td style="width:142.8pt;"> <p style="margin-left:0cm;"><strong><span style="color:#000000;">函数</span></strong></p> </td><td style="width:233.9pt;"> <p style="margin-left:0cm;"><strong><span style="color:#000000;">描述</span></strong></p> </td></tr><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;">1</p> </td><td style="vertical-align:top;width:142.8pt;"> <p style="margin-left:0cm;"><strong>readObjectFile</strong></p> </td><td style="vertical-align:top;width:233.9pt;"> <p style="margin-left:0cm;">读取对象文件</p> </td></tr><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;"><span style="color:#000000;">2</span></p> </td><td style="width:142.8pt;"> <p style="margin-left:0cm;"><strong><span style="color:#000000;">readFile</span></strong></p> </td><td style="width:233.9pt;"> <p style="margin-left:0cm;"><span style="color:#000000;">读取文件</span></p> </td></tr><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;">3</p> </td><td style="vertical-align:top;width:142.8pt;"> <p style="margin-left:0cm;"><strong>readImageFile</strong></p> </td><td style="vertical-align:top;width:233.9pt;"> <p style="margin-left:0cm;">用于读取2D图像文件</p> </td></tr><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;"><span style="color:#000000;">4</span></p> </td><td style="width:142.8pt;"> <p style="margin-left:0cm;"><strong><span style="color:#000000;">readHeightFieldFile</span></strong></p> </td><td style="width:233.9pt;"> <p style="margin-left:0cm;"></p> </td></tr><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;">5</p> </td><td style="vertical-align:top;width:142.8pt;"> <p style="margin-left:0cm;"><strong>readNodeFile</strong></p> </td><td style="vertical-align:top;width:233.9pt;"> <p style="margin-left:0cm;">打开3D模型文件，后缀名为.osg</p> </td></tr><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;"><span style="color:#000000;">6</span></p> </td><td style="width:142.8pt;"> <p style="margin-left:0cm;"><strong><span style="color:#000000;">readNodeFiles</span></strong></p> </td><td style="width:233.9pt;"> <p style="margin-left:0cm;"><span style="color:#000000;">打开多个</span><span style="color:#000000;">3D</span><span style="color:#000000;">模型文件，后缀名为</span><span style="color:#000000;">.osg</span></p> </td></tr><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;">7</p> </td><td style="vertical-align:top;width:142.8pt;"> <p style="margin-left:0cm;"><strong>readShaderFile</strong></p> </td><td style="vertical-align:top;width:233.9pt;"> <p style="margin-left:0cm;">读取着色器文件</p> </td></tr><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;"><span style="color:#000000;">8</span></p> </td><td style="width:142.8pt;"> <p style="margin-left:0cm;"><strong><span style="color:#000000;">readShaderFileWithFallback</span></strong></p> </td><td style="width:233.9pt;"> <p style="margin-left:0cm;"></p> </td></tr><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;">9</p> </td><td style="vertical-align:top;width:142.8pt;"> <p style="margin-left:0cm;"><strong>readScriptFile</strong></p> </td><td style="vertical-align:top;width:233.9pt;"> <p style="margin-left:0cm;">读取脚本文件</p> </td></tr><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;"><span style="color:#000000;">10</span></p> </td><td style="width:142.8pt;"> <p style="margin-left:0cm;"><strong><span style="color:#000000;">readRefObjectFile</span></strong></p> </td><td style="width:233.9pt;"> <p style="margin-left:0cm;"></p> </td></tr><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;"><span style="color:#000000;">11</span></p> </td><td style="width:142.8pt;"> <p style="margin-left:0cm;"><strong><span style="color:#000000;">readRefImageFile</span></strong></p> </td><td style="width:233.9pt;"> <p style="margin-left:0cm;"></p> </td></tr><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;"><span style="color:#000000;">12</span></p> </td><td style="width:142.8pt;"> <p style="margin-left:0cm;"><strong><span style="color:#000000;">readRefHeightFieldFile</span></strong></p> </td><td style="width:233.9pt;"> <p style="margin-left:0cm;"></p> </td></tr><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;"><span style="color:#000000;">13</span></p> </td><td style="width:142.8pt;"> <p style="margin-left:0cm;"><strong><span style="color:#000000;">readRefNodeFile</span></strong></p> </td><td style="width:233.9pt;"> <p style="margin-left:0cm;"></p> </td></tr><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;"><span style="color:#000000;">14</span></p> </td><td style="width:142.8pt;"> <p style="margin-left:0cm;"><strong><span style="color:#000000;">readRefShaderFile</span></strong></p> </td><td style="width:233.9pt;"> <p style="margin-left:0cm;"></p> </td></tr><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;"><span style="color:#000000;">15</span></p> </td><td style="width:142.8pt;"> <p style="margin-left:0cm;"><strong><span style="color:#000000;">readRefScriptFile</span></strong></p> </td><td style="width:233.9pt;"> <p style="margin-left:0cm;"></p> </td></tr></tbody></table>
- osgDB/writeFile：osg所有保存磁盘文件函数都在此头文件中
	<table border="1" cellspacing="0"><tbody><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;"><strong>序号</strong></p> </td><td style="width:142.8pt;"> <p style="margin-left:0cm;"><strong><span style="color:#000000;">函数</span></strong></p> </td><td style="width:233.9pt;"> <p style="margin-left:0cm;"><strong><span style="color:#000000;">描述</span></strong></p> </td></tr><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;">1</p> </td><td style="vertical-align:top;width:142.8pt;"> <p style="margin-left:0cm;"><strong>writeObjectFile</strong></p> </td><td style="vertical-align:top;width:233.9pt;"> <p style="margin-left:0cm;">将对象写入文件</p> </td></tr><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;"><span style="color:#000000;">2</span></p> </td><td style="width:142.8pt;"> <p style="margin-left:0cm;"><strong><span style="color:#000000;">writeImageFile</span></strong></p> </td><td style="width:233.9pt;"> <p style="margin-left:0cm;"><span style="color:#000000;">将数据写入</span><span style="color:#000000;">2D</span><span style="color:#000000;">图形文件</span></p> </td></tr><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;"><span style="color:#000000;">3</span></p> </td><td style="width:142.8pt;"> <p style="margin-left:0cm;"><strong><span style="color:#000000;">writeHeightFieldFile</span></strong></p> </td><td style="width:233.9pt;"> <p style="margin-left:0cm;"></p> </td></tr><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;"><span style="color:#000000;">4</span></p> </td><td style="width:142.8pt;"> <p style="margin-left:0cm;"><strong><span style="color:#000000;">writeNodeFile</span></strong></p> </td><td style="width:233.9pt;"> <p style="margin-left:0cm;"><span style="color:#000000;">将数据写入</span><span style="color:#000000;">3D</span><span style="color:#000000;">模型文件</span></p> </td></tr><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;"><span style="color:#000000;">5</span></p> </td><td style="width:142.8pt;"> <p style="margin-left:0cm;"><strong><span style="color:#000000;">writeShaderFile</span></strong></p> </td><td style="width:233.9pt;"> <p style="margin-left:0cm;"><span style="color:#000000;">将着色器写入文件</span></p> </td></tr><tr><td style="width:35.2pt;"> <p style="margin-left:0cm;"><span style="color:#000000;">6</span></p> </td><td style="width:142.8pt;"> <p style="margin-left:0cm;"><strong><span style="color:#000000;">writeScriptFile</span></strong></p> </td><td style="width:233.9pt;"> <p style="margin-left:0cm;"><span style="color:#000000;">将脚本写入文件</span></p> </td></tr></tbody></table>