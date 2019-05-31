## 常见问题
1.最近手误点击 logout 按钮，导致页面 VNCViewer 桌面卡死。

- 查找到当前桌面会话id

	$ vncserver -list 

- 找到杀死当前会话

	$ vncserver -kill :32

- 重启一个会话
	
	$ vncserver :32

2.