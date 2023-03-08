I cloned the enigma source and installed the dependencies listed, as well as the IDE and plugins, then I copy-pasted the given code into the newly created install.sh. As shown in the figure below.

![8](https://raw.githubusercontent.com/Origin-yy/Picture/main/202303061620859.png)!](/home/origin/Pictures/8.png)

But I didn't succeed. After I click Documentation on the far right of the toolbar, he will pop up the following window to report an error. 

![9](https://raw.githubusercontent.com/Origin-yy/Picture/main/202303061631809.png)!](/home/origin/Pictures/9.png)

The entire content of the Error Report is:

```bash

LateralGM Version: 1.8.234
Working Directory: /home/origin/enigma-dev/enigma-dev/lateralgm.jar

Operating System: Linux
Version: 6.1.12-arch1-1
Architecture: amd64

Java Name: OpenJDK 64-Bit Server VM
Java Vendor: N/A
Version: 19.0.2

Current Thread: AWT-EventQueue-0
Available processors (cores): 16
Free memory (bytes): 109439264
Maximum memory (bytes): 3628072960
Total memory available to JVM (bytes): 136314880

File system root: /
Total space (bytes): 83955703808
Free space (bytes): 43672080384
Usable space (bytes): 39360335872

Stack trace:
java.lang.UnsupportedOperationException: The BROWSE action is not supported on the current platform!
	at java.desktop/java.awt.Desktop.checkActionSupport(Desktop.java:381)
	at java.desktop/java.awt.Desktop.browse(Desktop.java:531)
	at org.lateralgm.main.Listener.actionPerformed(Listener.java:641)
	at java.desktop/javax.swing.AbstractButton.fireActionPerformed(AbstractButton.java:1972)
	at java.desktop/javax.swing.AbstractButton$Handler.actionPerformed(AbstractButton.java:2313)
	at java.desktop/javax.swing.DefaultButtonModel.fireActionPerformed(DefaultButtonModel.java:405)
	at java.desktop/javax.swing.DefaultButtonModel.setPressed(DefaultButtonModel.java:262)
	at java.desktop/javax.swing.plaf.basic.BasicButtonListener.mouseReleased(BasicButtonListener.java:279)
	at java.desktop/java.awt.AWTEventMulticaster.mouseReleased(AWTEventMulticaster.java:297)
	at java.desktop/java.awt.Component.processMouseEvent(Component.java:6620)
	at java.desktop/javax.swing.JComponent.processMouseEvent(JComponent.java:3398)
	at java.desktop/java.awt.Component.processEvent(Component.java:6385)
	at java.desktop/java.awt.Container.processEvent(Container.java:2266)
	at java.desktop/java.awt.Component.dispatchEventImpl(Component.java:4995)
	at java.desktop/java.awt.Container.dispatchEventImpl(Container.java:2324)
	at java.desktop/java.awt.Component.dispatchEvent(Component.java:4827)
	at java.desktop/java.awt.LightweightDispatcher.retargetMouseEvent(Container.java:4948)
	at java.desktop/java.awt.LightweightDispatcher.processMouseEvent(Container.java:4575)
	at java.desktop/java.awt.LightweightDispatcher.dispatchEvent(Container.java:4516)
	at java.desktop/java.awt.Container.dispatchEventImpl(Container.java:2310)
	at java.desktop/java.awt.Window.dispatchEventImpl(Window.java:2780)
	at java.desktop/java.awt.Component.dispatchEvent(Component.java:4827)
	at java.desktop/java.awt.EventQueue.dispatchEventImpl(EventQueue.java:775)
	at java.desktop/java.awt.EventQueue$4.run(EventQueue.java:720)
	at java.desktop/java.awt.EventQba shueue$4.run(EventQueue.java:714)
	at java.base/java.security.AccessController.doPrivileged(AccessController.java:399)
	at java.base/java.security.ProtectionDomain$JavaSecurityAccessImpl.doIntersectionPrivilege(ProtectionDomain.java:86)
	at java.base/java.security.ProtectionDomain$JavaSecurityAccessImpl.doIntersectionPrivilege(ProtectionDomain.java:97)
	at java.desktop/java.awt.EventQueue$5.run(EventQueue.java:747)
	at java.desktop/java.awt.EventQueue$5.run(EventQueue.java:745)
	at java.base/java.security.AccessController.doPrivileged(AccessController.java:399)
	at java.base/java.security.ProtectionDomain$JavaSecurityAccessImpl.doIntersectionPrivilege(ProtectionDomain.java:86)
	at java.desktop/java.awt.EventQueue.dispatchEvent(EventQueue.java:744)
	at java.desktop/java.awt.EventDispatchThread.pumpOneEventForFilters(EventDispatchThread.java:203)
	at java.desktop/java.awt.EventDispatchThread.pumpEventsForFilter(EventDispatchThread.java:124)
	at java.desktop/java.awt.EventDispatchThread.pumpEventsForHierarchy(EventDispatchThread.java:113)
	at java.desktop/java.awt.EventDispatchThread.pumpEvents(EventDispatchThread.java:109)
	at java.desktop/java.awt.EventDispatchThread.pumpEvents(EventDispatchThread.java:101)
	at java.desktop/java.awt.EventDispatchThread.run(EventDispatchThread.java:90)

```

About `java.lang.UnsupportedOperationException: The BROWSE action is not supported on the current platform!`.I did some searching.

Linux does not support the BROWSE action of the Desktop class: Open the default web browser and display a URL. Although Linux generally does not have a default browser, I have tried to set the default browser to chromium or firefox, but the problem is still not resolved.

"Manually open a web browser and copy-paste the URL into the address bar" might be a temporary solution, but I know little about java and I didn't find the URL.





