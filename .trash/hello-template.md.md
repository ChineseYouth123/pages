<%*
// 弹出输入框，默认值填"你好"，用户在弹窗里确认即可
const userInput = await tp.system.prompt("请输入你好：", "你好");
tR += "你输入了：" + userInput;
%>


```button
name 🖱️ 点击输入你好
type command
action Templater: 创建新笔记使用模板 - hello-template
color blue
```

