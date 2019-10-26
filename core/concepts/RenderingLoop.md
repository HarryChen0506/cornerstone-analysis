# Rendering Loop 渲染循环

> Viewport (e.g. windowing, pan, zoom, etc...) changes for Cornerstone Enabled Elements are updated through a rendering loop based on requestAnimationFrame.

利用现代浏览器的requestAnimationFrame（RAF）特性，可以高效的实现高帧数的canvas绘制。requestAnimationFrame相当于一个每隔16ms就会刷新的定时器，Viewport的参数比如缩放，位移等发生变化，Cornerstone则会让图像进行重新绘制，实现更新。

工作流程如下：

1. draw( ) 回调在RAF里面注册，这样就可以递归调用
2. draw( ) 函数将会在浏览器屏幕每帧进行刷新的时候调用。
3. 一旦被调用后，
  - 如果元素计划重渲染，则图像会被重渲染并且draw( ) 函数将会被重新注册到RAF里
  - 如果元素不打算重渲染，则不执行任何操作，但是draw( ) 函数仍然会被重新注册到RAF里
  - 如果元素disabled( )，draw 回调则不会被重注册到RAF, 并且结束渲染循环