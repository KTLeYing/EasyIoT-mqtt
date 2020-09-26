/*!
 * MindPlus
 * mpython
 *
 */
#include <MPython.h>
#include <DFRobot_Iot.h>
// 函数声明
void obloqMqttEventT0(String& message);
// 静态常量
const String topics[5] = {"wDHT5mFGg","s7V05iFMR","","",""};
const MsgHandleCb msgHandles[5] = {obloqMqttEventT0,NULL,NULL,NULL,NULL};
// 创建对象
DFRobot_Iot myIot;


// 主程序开始
void setup() {
	mPython.begin();
	myIot.setMqttCallback(msgHandles);
	display.setCursorLine(1);
	display.printLine("程序开始");
	myIot.wifiConnect("MEIZU 16th", "mazhenle520");
	while (!myIot.wifiStatus()) {yield();}
	rgb.write(0, 0x0000FF);
	display.setCursorLine(2);
	display.printLine("Wi-Fi连接成功");
	display.setCursorLine(2);
	display.printLine(myIot.getWiFiLocalIP());
	myIot.init("iot.dfrobot.com.cn","5-iocmFMR","","5-iocmKGRz",topics,1883);
	myIot.connect();
	while (!myIot.connected()) {yield();}
	display.setCursorLine(2);
	display.printLine("MQTT连接成功");
}
void loop() {
	if ((buttonA.isPressed())) {
		myIot.publish(topic_1, "NBA!牛逼！");
		display.setCursorLine(1);
		display.printLine("MQTT发送成功");
	}
}


// 事件回调函数
void obloqMqttEventT0(String& message) {
	display.fillScreen(0);
	display.setCursorLine(4);
	display.printLine(message);
	rgb.write(1, 0xFFFF00);
}
