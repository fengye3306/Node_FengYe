# ccViewer  
*CloudCompare/libs/qCC_glWindow* 



### 枚举运算符

> 枚举内部使用位运算符

```c++
//! Interaction flags (mostly with the mouse)
	// 交互标志(主要是鼠标相关)
	enum INTERACTION_FLAG	
	{
		//no interaction
		//无交互
		INTERACT_NONE = 0,  

		//camera interactions
		//摄像机交互
		INTERACT_ROTATE          = 1,    // 旋转
		INTERACT_PAN             = 2,    // 平移
		INTERACT_CTRL_PAN        = 4,    // 控制平移
		INTERACT_ZOOM_CAMERA     = 8,    // 缩放摄像机
		INTERACT_2D_ITEMS        = 16,   //labels, etc.// 2D项目(标签.等)
		INTERACT_CLICKABLE_ITEMS = 32,   //hot zone	// 可点击项目（热点区）

		//options / modifiers
		INTERACT_TRANSFORM_ENTITIES = 64,
		
		//signals
		INTERACT_SIG_RB_CLICKED      =  128, //right button clicked
		INTERACT_SIG_LB_CLICKED      =  256, //left button clicked
		INTERACT_SIG_MOUSE_MOVED     =  512, //mouse moved (only if a button is clicked)
		INTERACT_SIG_BUTTON_RELEASED = 1024, //mouse button released
		INTERACT_SIG_MB_CLICKED      = 2048, //middle button clicked
		INTERACT_SEND_ALL_SIGNALS    = INTERACT_SIG_RB_CLICKED | INTERACT_SIG_LB_CLICKED | INTERACT_SIG_MB_CLICKED | INTERACT_SIG_MOUSE_MOVED | INTERACT_SIG_BUTTON_RELEASED,

		/*default modes
		*/ 
		// 仅平移模式
		MODE_PAN_ONLY 
            = INTERACT_PAN | INTERACT_ZOOM_CAMERA | INTERACT_2D_ITEMS | INTERACT_CLICKABLE_ITEMS,	
		// 变换摄像机模式
		MODE_TRANSFORM_CAMERA 
            = INTERACT_ROTATE | MODE_PAN_ONLY,
		// 变换实体模式
		MODE_TRANSFORM_ENTITIES 
            = INTERACT_ROTATE | INTERACT_PAN | INTERACT_ZOOM_CAMERA | INTERACT_TRANSFORM_ENTITIES | INTERACT_CLICKABLE_ITEMS,
	};
```

> 枚举外的聚和变量  

```c++

	/**
	 * 枚举组合，它可以包含多个 INTERACTION_FLAG 的值。这样，
	 * 你可以在一个变量中存储和处理多个交互标志。
	 * 
	 * // 例如
	 * INTERACTION_FLAGS flags = INTERACT_ROTATE | INTERACT_PAN | INTERACT_ZOOM_CAMERA;
	 */
	Q_DECLARE_FLAGS(INTERACTION_FLAGS, INTERACTION_FLAG)
```

## 文档   



### ccGLWindowInterface  
* 构造函数 `ccGLWindowInterface` (QObject* parent = nullptr, bool silentInitialization = false)
* 析构函数 `~ccGLWindowInterface` ()
* 返回窗口是否为立体显示 `isStereo` () const = 0
* 获取像素比例 `getDevicePixelRatio` () const = 0
* 获取字体 `getFont` () const = 0
* 获取 OpenGL 上下文对象 `getOpenGLContext` () const = 0
* 设置窗口光标样式 `setWindowCursor` (const QCursor&) = 0
* 将当前上下文设置为当前线程的主要 OpenGL 上下文 `doMakeCurrent` () = 0
* 获取作为 QObject 对象的实例 `asQObject` () = 0
* 获取作为 QObject 对象的实例（常量版本） `asQObject` () const = 0
* 获取窗口标题 `getWindowTitle` () const = 0
* 捕获鼠标输入 `doGrabMouse` () = 0
* 释放鼠标输入 `doReleaseMouse` () = 0
* 将全局坐标映射到窗口内的局部坐标 `doMapFromGlobal` (const QPoint &) const = 0
* 显示窗口并最大化 `doShowMaximized` () = 0
* 调整窗口大小 `doResize` (int w, int h) = 0
* 调整窗口大小 `doResize` (const QSize &) = 0
* 抓取当前缓冲区作为QImage返回 `doGrabFramebuffer` () = 0
* 刷新视图 `refresh` (bool only2D = false) override
* 重绘视图 `redraw` (bool only2D = false, bool resetLOD = true) override
* 设置 'scene graph' 根节点 `setSceneDB` (ccHObject* root)
* 返回当前 'scene graph' 根节点 `getSceneDB` ()
* 渲染文本 `renderText` (int x, int y, const QString & str, uint16_t uniqueID = 0, const QFont & font = QFont())
* 渲染文本 `renderText` (double x, double y, double z, const QString & str, const QFont & font = QFont())
* 标记需要刷新 `toBeRefreshed` () override
* 使视口无效 `invalidateViewport` () override
* 使3D层过时 `deprecate3DLayer` () override
* 显示3D标签 `display3DLabel` (const QString& str, const CCVector3& pos3D, const ccColor::Rgba* color = nullptr, const QFont& font = QFont()) override
* 显示文本 `displayText` (QString text, int x, int y, unsigned char align = ALIGN_DEFAULT, float bkgAlpha = 0.0f, const ccColor::Rgba* color = nullptr, const QFont* font = nullptr) override
* 获取文本显示字体 `getTextDisplayFont` () const override
* 获取标签显示字体 `getLabelDisplayFont` () const override
* 获取视口参数 `getViewportParameters` () const override
* 转换为居中的GL坐标 `toCenteredGLCoordinates` (int x, int y) const override
* 转换为角落的GL坐标 `toCornerGLCoordinates` (int x, int y) const override
* 设置投影视口 `setupProjectiveViewport` (const ccGLMatrixd& cameraMatrix, float fov_deg = 0.0f, bool viewerBasedPerspective = true, bool bubbleViewMode = false) override
* 即将被移除 `aboutToBeRemoved` (ccDrawableObject* entity) override
* 获取GL相机参数 `getGLCameraParameters` (ccGLCameraParameters& params) override
* 显示状态消息 `displayNewMessage` (const QString& message, MessagePosition pos, bool append=false, int displayMaxDelay_sec = 2, MessageType type = CUSTOM_MESSAGE)
* 激活太阳光 `setSunLight` (bool state)
* 切换太阳光状态 `toggleSunLight` ()
* 返回太阳光是否启用 `sunLightEnabled` () const
* 激活自定义光源 `setCustomLight` (bool state)
* 切换自定义光源状态 `toggleCustomLight` ()
* 返回自定义光源是否启用 `customLightEnabled` () const
* 返回自定义光源的当前位置 `getCustomLightPosition` () const
* 设置自定义光源当前位置 `setCustomLightPosition` (const CCVector3f& pos)
* 设置枢轴可见性 `setPivotVisibility` (PivotVisibility vis)
* 返回枢轴可见性 `getPivotVisibility` () const
* 显示或隐藏枢轴符号 `showPivotSymbol` (bool state)
* 设置枢纽点 `setPivotPoint` (const CCVector3d& P, bool autoUpdateCameraPos = false, bool verbose = false)
* 设置相机位置 `setCameraPos` (const CCVector3d& P)
* 位移相机 `moveCamera` (CCVector3d& v)
* 设置OpenGL相机焦距，实现给定宽度 `setCameraFocalToFitWidth` (double width)
* 设置焦距 `setFocalDistance` (double focalDistance)
* 设置透视状态/模式 `setPerspectiveState` (bool state, bool objectCenteredView)
* 切换透视模式 `togglePerspective` (bool objectCentered)
* 返回透视模式 `getPerspectiveState` (bool& objectCentered) const
* 返回是否启用了对象透视模式 `objectPerspectiveEnabled` () const
* 返回是否启用了观察者透视模式 `viewerPerspectiveEnabled` () const
* 设置泡泡视图模式状态 `setBubbleViewMode` (bool state)
* 返回泡泡视图模式是否启用 `bubbleViewModeEnabled` () const
* 设置泡泡视图视场 `setBubbleViewFov` (float fov_deg)
* 居中并缩放到给定的边界框 `updateConstellationCenterAndZoom` (const ccBBox* boundingBox = nullptr)
* 返回可见对象的边界框 `getVisibleObjectsBB` (ccBBox& box) const
* 旋转基本视图矩阵 `rotateBaseViewMat` (const ccGLMatrixd& rotMat)
* 返回基本视图矩阵 `getBaseViewMat` ()
* 设置基本视图矩阵 `setBaseViewMat` (ccGLMatrixd& mat)
* 设置相机到预定义视图 `setView` (CC_VIEW_ORIENTATION orientation, bool redraw = true)
* 设置相机到自定义视图 `setCustomView` (const CCVector3d& forward, const CCVector3d& up, bool forceRedraw = true)
* 设置当前交互模式 `setInteractionMode` (INTERACTION_FLAGS flags)
* 返回当前交互模式 `getInteractionMode` () const
* 设置当前选择模式 `setPickingMode` (PICKING_MODE mode = DEFAULT_PICKING, Qt::CursorShape defaultCursorShape = Qt::ArrowCursor)
* 返回当前选择模式 `getPickingMode` () const
* 锁定选择模式 `lockPickingMode` (bool state)
* 返回选择模式是否锁定 `isPickingModeLocked` () const
* 指定该3D窗口是否可以被用户关闭 `setUnclosable` (bool state)
* 返回上下文信息 `getContext` (CC_DRAW_CONTEXT& context)
* 设置点大小 `setPointSize` (float size, bool silent = false)
* 设置线宽 `setLineWidth` (float width, bool silent = false)
* 返回当前字体大小 `getFontPointSize` () const
* 返回标签的当前字体大小 `getLabelFontPointSize` () const
* 返回窗口自身的数据库 `getOwnDB` ()
* 向窗口自身的数据库添加实体 `addToOwnDB` (ccHObject* obj, bool noDependency = true)
* 从窗口自身的数据库中移除实体 `removeFromOwnDB` (ccHObject* obj)
* 设置视口参数 `setViewportParameters` (const ccViewportParameters& params)
* 设置当前相机视场 `setFov` (float fov)
* 返回当前视场 `getFov` () const
* 启用或禁用近裁剪平面和远裁剪平面 `setClippingPlanesEnabled` (bool enabled)
* 返回是否启用近裁剪平面和远裁剪平面 `clippingPlanesEnabled` () const
* 设置近裁剪平面深度 `setNearClippingPlaneDepth` (double depth)
* 设置远裁剪平面深度 `setFarClippingPlaneDepth` (double depth)
* 使当前可视化状态无效 `invalidateVisualization` ()
* 将屏幕渲染为图像 `renderToImage` (float zoomFactor = 1.0f, bool dontScaleFeatures = false, bool renderOverlayItems = false, bool silent = false)
* 将屏幕渲染为文件 `renderToFile` (QString filename, float zoomFactor = 1.0f, bool dontScaleFeatures = false, bool renderOverlayItems = false)
* 设置着色器路径 `SetShaderPath` (const QString &path)
* 获取着色器路径 `GetShaderPath` ()
* 设置着色器 `setShader` (ccShader* shader)
* 设置GL过滤器 `setGlFilter` (ccGlFilter* filter)
* 获取GL过滤器 `getGlFilter` ()
* 获取GL过滤器（常量版本） `getGlFilter` () const
* 返回是否启用着色器 `areShadersEnabled` () const
* 返回是否启用GL过滤器 `areGLFiltersEnabled` () const
* 计算屏幕上实际像素大小 `computeActualPixelSize` () const
* 返回是否支持颜色渐变着色器 `hasColorRampShader` () const
* 返回是否允许矩形选择 `isRectangularPickingAllowed` () const
* 设置是否允许矩形选择 `setRectangularPickingAllowed` (bool state)
* 获取当前显示参数（常量版本） `getDisplayParameters` () const
* 设置当前显示参数 `setDisplayParameters` (const ccGui::ParamStruct& params, bool thisWindowOnly = false)
* 返回是否为此窗口覆盖了显示参数 `hasOverriddenDisplayParameters` () const
* 设置选择半径 `setPickingRadius` (int radius)
* 返回当前选择半径 `getPickingRadius` () const
* 设置是否显示覆盖实体 `displayOverlayEntities` (bool showScale, bool showTrihedron)
* 返回是否显示比例尺 `scaleIsDisplayed` () const
* 返回是否显示三棱柱 `trihedronIsDisplayed` () const
* 计算三棱柱的大小（以像素为单位） `computeTrihedronLength` () const
* 返回GL过滤器横幅的高度 `getGlFilterBannerHeight` () const
* 返回用于显示颜色渐变的垂直区域的范围 `computeColorRampAreaLimits` (int& yStart, int& yStop) const
* 将2D点反向投影到3D三角形上 `backprojectPointOnTriangle` (const CCVector2i& P2D, const CCVector3& A3D, const CCVector3& B3D, const CCVector3& C3D)
* 返回唯一ID `getUniqueID` () const
* 返回小部件宽度（以像素为单位） `qtWidth` () const = 0
* 返回小部件高度（以像素为单位） `qtHeight` () const = 0
* 返回小部件大小（以像素为单位） `qtSize` () const = 0
* 返回OpenGL上下文宽度 `glWidth` () const
* 返回OpenGL上下文高度 `glHeight` () const
* 返回OpenGL上下文大小 `glSize` () const
* 返回是否启用了LOD `isLODEnabled` () const
* 启用或禁用LOD `setLODEnabled` (bool state, bool autoDisable = false)
* 切换（独占）全屏模式 `toggleExclusiveFullScreen` (bool state)
* 返回窗口是否处于独占全屏模式 `exclusiveFullScreen` () const
* 显示调试信息 `enableDebugTrace` (bool state)
* 切换调试信息显示 `toggleDebugTrace` ()
* 启用立体显示模式 `enableStereoMode` (const StereoParams& params)
* 禁用立体显示模式 `disableStereoMode` ()
* 返回立体显示模式是否启用 `stereoModeIsEnabled` () const
* 返回当前立体模式参数 `getStereoParams` () const
* 设置是否显示光标下方点的坐标 `showCursorCoordinates` (bool state)
* 返回是否显示光标下方点的坐标 `cursorCoordinatesShown` () const
* 切换自动设置枢轴点到屏幕中心 `setAutoPickPivotAtCenter` (bool state)
* 返回枢轴点是否自动设置到屏幕中心 `autoPickPivotAtCenter` () const
* 锁定旋转轴 `lockRotationAxis` (bool state, const CCVector3d& axis)
* 返回旋转轴是否锁定 `isRotationAxisLocked` () const
* 应用1:1全局缩放 `zoomGlobal` ()
* 处理鼠标滚轮事件 `onWheelEvent` (float wheelDelta_deg)
* 测试帧率 `startFrameRateTest` ()
* 请求更新显示 `requestUpdate` () = 0
* 返回信号发射器（常量版本） `signalEmitter` () const
* 返回信号发射器 `signalEmitter` ()
* 创建ccGLWindowInterface实例 `Create` (ccGLWindowInterface*& window, QWidget*& widget, bool stereoMode = false, bool silentInitialization = false)
* 从QWidget创建ccGLWindowInterface实例 `FromWidget` (QWidget* widget)
* 从QObject创建ccGLWindowInterface实例 `FromEmitter` (QObject* object)
* 从QObject创建ccGLWindowInterface实例 `FromQObject` (QObject* object)
* 返回是否支持立体显示 `StereoSupported` ()
* 设置是否支持立体显示 `SetStereoSupported` (bool state)
* 测试是否支持立体显示 `TestStereoSupport` (bool forceRetest = false)
* 返回是否支持四缓冲 `isQuadBufferSupported` () const
* 设置显示比例 `setDisplayScale` (const CCVector2d& scale)
* 返回显示比例 `getDisplayScale` () const
* 返回OpenGL函数集 `functions` () const = 0
* 处理屏幕调整大小事件 `onResizeGL` (int w, int h)
* 设置OpenGL视口 `setGLViewport` (const QRect& rect)
* 设置OpenGL视口（快捷方式） `setGLViewport` (int x, int y, int w, int h)
* 初始化OpenGL上下文 `initialize` ()
* 释放所有纹理、GL列表等 `uninitializeGL` ()
* 渲染方法 `doPaintGL` ()
* 渲染方法自定义初始化步骤 `initPaintGL` () = 0
* 交换OpenGL缓冲区（如有必要） `swapGLBuffers` () = 0
* 初始化过程开始时调用 `preInitialize` (bool& firstTime)
* 初始化过程结束时调用 `postInitialize` (bool firstTime)
* 处理鼠标按下事件 `processMousePressEvent` (QMouseEvent event)
* 处理鼠标双击事件 `processMouseDoubleClickEvent` (QMouseEvent event)
* 处理鼠标移动事件 `processMouseMoveEvent` (QMouseEvent event)
* 处理鼠标释放事件 `processMouseReleaseEvent` (QMouseEvent event)
* 处理滚轮事件 `processWheelEvent` (QWheelEvent event)
* 启用立体显示模式（高级模式） `enableStereoMode` (const StereoParams& params, bool needSecondFBO, bool needAutoRefresh)
* 处理拖动事件 `doDragEnterEvent` (QDragEnterEvent event)
* 处理放置事件 `doDropEvent` (QDropEvent event)
* 响应快速选择信号 `onItemPickedFast` (ccHObject pickedEntity, int pickedItemIndex, int x, int y)
* 检查计划的重绘 `checkScheduledRedraw` ()
* 执行标准选择 `doPicking` ()
* 绑定FBO或释放当前FBO `bindFBO` (ccFrameBufferObject* fbo)
* 返回宽度（像素） `width` () const = 0
* 返回高度（像素） `height` () const = 0
* 返回大小（像素） `size` () const = 0
* 返回当前OpenGL视图矩阵 `getModelViewMatrix` ()
* 返回当前OpenGL投影矩阵 `getProjectionMatrix` ()
* 处理可点击项目 `processClickableItems` (int x, int y)
* 设置当前字体大小 `setFontPointSize` (int pixelSize)
* 返回默认Qt FBO `defaultQtFBO` () const = 0
* 计算视图矩阵 `computeModelViewMatrix` () const
* 计算投影矩阵 `computeProjectionMatrix` (bool withGLfeatures, ProjectionMetrics* metrics = nullptr, double* eyeOffset = nullptr) const
* 更新视图矩阵 `updateModelViewMatrix` ()
* 更新投影矩阵 `updateProjectionMatrix` ()
* 设置标准正交中心 `setStandardOrthoCenter` ()
* 设置标准正交角落 `setStandardOrthoCorner` ()
* 启用太阳光 `glEnableSunLight` ()
* 禁用太阳光 `glDisableSunLight` ()
* 启用自定义光源 `glEnableCustomLight` ()
* 禁用自定义光源 `glDisableCustomLight` ()
* 绘制自定义光源 `drawCustomLight` ()
* 开始选择过程 `startPicking` (PickingParameters& params)
* 执行OpenGL选择（基于颜色） `startOpenGLPicking` (const PickingParameters& params)
* 启动基于CPU的点选择 `startCPUBasedPointPicking` (const PickingParameters& params)
* 处理选择结果并发送相应信号 `processPickingResult` (const PickingParameters& params, ccHObject* pickedEntity, int pickedItemIndex, const CCVector3* nearestPoint = nullptr, const CCVector3d* nearestPointBC = nullptr, const std::unordered_set<int>* selectedIDs = nullptr)
* 更新当前活动项目列表 `updateActiveItemsList` (int x, int y, bool extendToSelectedLabels = false)
* 初始化FBO `initFBO` (int w, int h)
* 安全初始化FBO `initFBOSafe` (ccFrameBufferObject* &fbo, int w, int h)
* 释放任何活动的FBO `removeFBO` ()
* 安全释放任何活动的FBO `removeFBOSafe` (ccFrameBufferObject* &fbo)
* 初始化活动GL过滤器 `initGLFilter` (int w, int h, bool silent = false)
* 释放活动GL过滤器 `removeGLFilter` ()
* 将给定（鼠标）位置转换为方向 `convertMousePositionToOrientation` (int x, int y)
* 绘制“热区” `drawClickableItems` (int xStart, int& yStart)
* 绘制十字架 `drawCross` ()
* 绘制三棱柱 `drawTrihedron` ()
* 绘制比例尺 `drawScale` (const ccColor::Rgbub& color)
* 禁用当前LOD渲染循环 `stopLODCycle` ()
* 安排完全重绘 `scheduleFullRedraw` (unsigned maxDelay_ms)
* 取消任何计划的重绘 `cancelScheduledRedraw` ()
* 记录GL错误 `LogGLError` (GLenum error, const char* context)
* 记录GL错误（快捷方式） `logGLError` (const char* context) const
* 切换自动刷新模式 `toggleAutoRefresh` (bool state, int period_ms = 0)
* 返回给定像素位置的相对深度值 `getGLDepth` (int x, int y, bool extendToNeighbors = false, bool usePBO = false)
* 返回点击像素的近似3D位置 `getClick3DPos` (int x, int y, CCVector3d& P3D, bool usePBO)
* 计算默认增量 `computeDefaultIncrement` () const
* 处理事件 `processEvents` (QEvent* evt)
* 停止帧率测试 `stopFrameRateTest` ()
* 返回帧率测试是否正在进行 `isFrameRateTestInProgress` ()
* 更新帧率测试 `updateFrameRateTest` ()