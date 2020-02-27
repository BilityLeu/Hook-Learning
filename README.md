# Hook-Learning
Hook学习笔记
# 1. WH_CALLWNDPROC（常量为4）和WH_CALLWNDPROCRET（常量为12）钩子
WH_CALLWNDPROC和WH_CALLWNDPROCRET Hooks使你可以监视发送到窗口过程的消息。系统在消息发送到接收窗口过程之前调用WH_CALLWNDPROC Hook子程，并且在窗口过程处理完消息之后调用WH_CALLWNDPROCRET Hook子程。
    WH_CALLWNDPROCRET Hook传递指针到CWPRETSTRUCT结构，再传递到Hook子程。
    CWPRETSTRUCT结构包含了来自处理消息的窗口过程的返回值，同样也包括了与这个消息关联的消息参数。

# 2.WH_CBT 钩子（常量为5）
在以下事件之前，系统都会调用WH_CBTHook子程，这些事件包括：
- 激活，建立，销毁，最小化，最大化，移动，改变尺寸等窗口事件；
- 完成系统指令；
- 来自系统消息队列中的移动鼠标，键盘事件；
- 设置输入焦点事件；
- 同步系统消息队列事件。
- Hook子程的返回值确定系统是否允许或者防止这些操作中的一个。

# 3. WH_DEBUG 钩子（常量为9）
在系统调用系统中与其他Hook关联的Hook子程之前，系统会调用WH_DEBUG Hook子程。你可以使用这个Hook来决定是否允许系统调用与其他Hook关联的Hook子程。

# 4. WH_FOREGROUNDIDLE 钩子（常量为11）
当应用程序的前台线程处于空闲状态时，可以使用WH_FOREGROUNDIDLE Hook执行低优先级的任务。当应用程序的前台线程大概要变成空闲状态时，系统就会调用WH_FOREGROUNDIDLE Hook子程。

# 5. WH_GETMESSAGE 钩子（常量为3）
应用程序使用WH_GETMESSAGE Hook来监视从GetMessage orPeekMessage函数返回的消息。你可以使用WH_GETMESSAGE Hook去监视鼠标和键盘输入，以及其他发送到消息队列中的消息。

# 6. WH_JOURNALPLAYBACK 钩子（常量为1）
WH_JOURNALPLAYBACK Hook使应用程序可以插入消息到系统消息队列。可以使用这个Hook回放通过使用WH_JOURNALRECORD Hook记录下来的连续的鼠标和键盘事件。只要WH_JOURNALPLAYBACK Hook已经安装，正常的鼠标和键盘事件就是无效的。
WH_JOURNALPLAYBACK Hook是全局Hook，它不能象线程特定Hook一样使用。
WH_JOURNALPLAYBACK Hook返回超时值，这个值告诉系统在处理来自回放Hook当前消息之前需要等待多长时间（毫秒）。这就使Hook可以控制实时事件的回放。
WH_JOURNALPLAYBACK是system-wide local hooks，它们不会被注射到任何行程位址空间。

# 7. WH_JOURNALRECORD 钩子（常量为0）
WH_JOURNALRECORD Hook用来监视和记录输入事件。典型的，可以使用这个Hook记录连续的鼠标和键盘事件，然后通过使用WH_JOURNALPLAYBACK Hook来回放。
WH_JOURNALRECORD Hook是全局Hook，它不能象线程特定Hook一样使用。
WH_JOURNALRECORD是system-wide local hooks，它们不会被注射到任何行程位址空间。

# 8. WH_KEYBOARD 钩子（常量为2）
在应用程序中，WH_KEYBOARD Hook用来监视WM_KEYDOWN andWM_KEYUP消息，这些消息通过GetMessage orPeekMessage function返回。可以使用这个Hook来监视输入到消息队列中的键盘消息。

# 9. WH_KEYBOARD_LL 钩子（常量为13）
WH_KEYBOARD_LL Hook监视输入到线程消息队列中的键盘消息。

# 10. WH_MOUSE 钩子（常量为7）
WH_MOUSE Hook监视从GetMessage 或者 PeekMessage 函数返回的鼠标消息。使用这个Hook监视输入到消息队列中的鼠标消息。

# 11. WH_MOUSE_LL 钩子（常量为14）
WH_MOUSE_LL Hook监视输入到线程消息队列中的鼠标消息。

# 12. WH_MSGFILTER（常量为-1）和 WH_SYSMSGFILTER 钩子（常量为6）
WH_MSGFILTER 和 WH_SYSMSGFILTER Hooks使我们可以监视菜单，滚动条，消息框，对话框消息并且发现用户使用ALT+TAB or ALT+ESC 组合键切换窗口。WH_MSGFILTER Hook只能监视传递到菜单，滚动条，消息框的消息，以及传递到通过安装了Hook子程的应用程序建立的对话框的消息。WH_SYSMSGFILTER Hook监视所有应用程序消息。
WH_MSGFILTER 和 WH_SYSMSGFILTER Hooks使我们可以在模式循环期间过滤消息，这等价于在主消息循环中过滤消息。  
通过调用CallMsgFilterfunction可以直接的调用WH_MSGFILTER Hook。通过使用这个函数，应用程序能够在模式循环期间使用相同的代码去过滤消息，如同在主消息循环里一样。

# 13. WH_SHELL 钩子（常量为10）
外壳应用程序可以使用WH_SHELL Hook去接收重要的通知。当外壳应用程序是激活的并且当顶层窗口建立或者销毁时，系统调用WH_SHELL Hook子程。
WH_SHELL 共有５钟情况：
- 只要有个top-level、unowned 窗口被产生、起作用、或是被摧毁；
- 当Taskbar需要重画某个按钮；
- 当系统需要显示关于Taskbar的一个程序的最小化形式；
- 当目前的键盘布局状态改变；
- 当使用者按Ctrl+Esc去执行Task Manager（或相同级别的程序）。
按照惯例，外壳应用程序都不接收WH_SHELL消息。所以，在应用程序能够接收WH_SHELL消息之前，应用程序必须调用SystemParametersInfofunction注册它自己。
