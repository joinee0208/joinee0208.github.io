---
layout: post
title: DLNA协议分析及应用
categories: 网络协议
original: true
description: DLNA协议分析及应用
keywords: DLNA
typora-root-url: ..\..
---
[1]:/images/dlna/dlna_overview.png
[2]:/images/dlna/device_overview.png
[3]:/images/dlna/api_overview.png
[4]:/images/dlna/metamodel_overview.png
[5]:/images/dlna/upnp_overview.png

## DLNA简介

DLNA的全称是DIGITAL LIVING NETWORK ALLIANCE(数字生活网络联盟)， 其宗旨是Enjoy your music, photos and videos, anywhere anytime， DLNA(Digital Living Network Alliance) 由索尼、英特尔、微软等发起成立、旨在解决个人PC，消费电器，移动设备在内的无线网络和有线网络的互联互通，使得数字媒体和内容服务的无限制的共享和增长成为可能，目前成员公司已达280多家。	

## DLNA架构

![DLNA ARCHITECTURE][1]

DLNA是基于UPnP协议，DLNA整个发现,控制,事件订阅部分都是由Upnp Device Architecture协议所定义，因此有必要了解下UPnP协议。

![img][5]

UPnP协议结构最底层的TCP/IP协议是UPnP协议结构的基础。IP层用于数据的发送与接收。对于需要可靠传送的信息,使用TCP进行传送, 反之则使用UDP。UPnP对网络物理设备没有要求,可以使用以太网、无线网、IEEE1394、红外进行连接, 只要支持IP协议即可。

构建在TCP/IP协议之上的是HTTP协议及其变种,这一部分是UPnP协议的核心部分, 所有UPnP消息都被封装在HTTP协议及其变种之中。HTTP协议的变种是HTTPU和HTTPMU, 这些协议的格式沿袭了HTTP协议,只不过与HTTP协议不同的是它们通过UDP而不是TCP来发送消息,并且可以用于多播通信。

UPnP包含以下协议：

* SSDP协议  

	简单服务发现协议（Simple Service Discovery Protocol：SSDP)，内建在HTTPU/HTTPMU 里，定义如何让网络上有的服务被发现的协议。包括控制点如何发现网络上有哪些服务，并取得这些服务的资讯，还有装置本身宣告他提供哪些服务。该协议运用在UPnP工作流程的设备发现部分。

* SOAP协议

	简单对象访问协议( Simple Object Access Protocol) 定义了可扩展标记语言(XML ) 和HTTP 的使用来执行远程调用,包括控制点如何发送命令消息给设备，及设备接收到命令消息后如何发送响应消息给控制点。该协议运用在UPnP工作流程的设备控制部分。

* GENA协议

	一般事件通知架构(Generic Event Notification Architecture：GENA)定义在控制点想要监听设备的某个服务状态变量的状况时，控制点如何传送订阅讯息并如何接收通知讯息用的。该协议运用在UPnP工作流程的事件订阅部分。



## UPnP组件

图为组件之间的关系：

![img][2]

* 服务(Service)

	在UPNP网络中，最新的控制控制单元就是服务，服务描述的设备在不同的情况下的活动和设备的状态。例如，时钟服务可以表述为时间变化(状态变化)，当前的时间(时间状态)以及设置时间和读取时间两个活动，通过这两个活动，你就可以控制服务。
* 设备(Device)
	
	UPnP网络中定义的设备具有很广泛的含义，各种各样的家电、电脑外设、智能设备、无线设备、个人电脑等等都可以成为其中一员。一个UPnP设备可以是多个服务的载体和多个子设备的嵌套集。例如一台印表机有提供列印这样的服务；一台电视有提供收讯的服务，这些都属于设备。
* 控制点(ControlPoint)
	
	在UPnP网络中，控制点指的是可以发现并控制其它设备的控制设备。在UPnP网络中，设备可以和控制点合并。也就是说，同一个设备，可以同时具有设备的功能和控制点的功能，即可以作为设备提供服务，也可以作为控制点发现和控制其它设备。



## UPnP工作流程

1. 寻址 

	UPnP 网络互连的基础是IP协议,因此必须先获取一个有效的IP地址。

2. 发现

	在将一个设备添加到网络上之后，UPnP 发现协议允许该设备向网络中的控制点宣告其服务。同
	样，当一个控制点被添加到网络后，UPnP 发现协议允许该控制点在网上搜索感
	兴趣的设备。两种情况下的根本信息交换均为一个发现消息，包含有关该设备或
	其服务之一的一些基础信息（例如其类型、标识符和指向更详细信息的一个指
	针）。UPnP 发现协议基于简单服务发现协议（SSDP）。

3. 描述

	控制点在发现一个设备之后仍然对其知之甚
	少。为了使控制点了解到更多关于设备及其能力的信息或与设备进行交互，则控
	制点必须取得来自该设备在发现消息中所提供之 URL 的设备描述。设备可能包
	含其它逻辑设备，以及功能单元或服务 。对于设备的 UPnP 描述通过 XML 来表
	达，并包括诸如模型名称和号码、序列号、制造商名称和厂商专门网站 URL 等
	专门针对厂商的制造商信息。该描述还包括一列任意的嵌入式设备或服务，以及
	用于控制、事件触发和展示的URL。对于每项服务，此描述均包括一列命令或
	动作，而服务（参数或变量）对于每个动作做出响应；针对服务的描述还包括一
	列变量；这些变量模型化服务在运行时的状态，并通过数据类型、范围和事件特
	征进行描述。以下关于描述的部分说明了设备如何被描述，以及这些描述如何被
	控制点取得。

4. 控制

	当一个控制点取得设备描述后，该控制点可
	将动作发至一个设备的服务。为此，控制点将一条适当的控制消息发至服务的控
	制 URL（在设备描述中提供）。控制消息同样利用简单对象访问协议（SOAP）
	通过 XML 来表达。类似于功能调用，该服务针对控制消息返回了所有的专门动
	作取值。动作的效果可以通过描述服务运行时状态的变量进行描述。以下关于控
	制的部分说明了有关动作、状态变量以及控制消息格式的描述。
 
5. 事件

	针对服务的 UPnP 描述包括一个服务响应
	的动作列表，以及一个对服务器运行时状态进行展示的变量列表。在这些变量变
	更时服务会发布更新，一个控制点可以预订接收此信息。服务通过发送事件消息
	来发布更新。事件消息包含一个或多个状态变量名和这些变量的当前值。这些消
	息同样通过 XML 来表达，并采用通用事件通知架构（GENA）格式。当控制点
	首次预定时，会发送一个特殊的初始事件消息；此事件消息包含所有事件变量的
	名称和值，并允许订阅者对服务状态模式进行初始化。为了支持拥有多个控制点
	的环境，事件触发设计用于将任何动作的效果通知所有控制点。因此，所有订阅
	者均会收到全部的事件消息。订阅者收到关于所有已变更事件变量的事件消息，
	此事件消息无论状态变量为何改变都被发送（由于响应一个要求动作，或由于服
	务建模状态的变更）。以下关于事件触发的部分说明了事件消息的预订和格式。

6. 展示

	如果设备有用于展示的 URL，那么控制点
	就可以通过此 URL 取得一个页面，在浏览器中加载该页面，并且根据页面的功
	能，支持用户控制设备和/或浏览设备状态。每一项完成的程度取决于展示页面
	和设备的具体功能。以下关于展示的部分说明了关于取得一个展示页面的协议。

## UPnP消息描述

### UPnP搜索

	M-SEARCH * HTTP/1.1
	Man: "ssdp:discover" //固定
	Mx: 10 //等待时长
	Host: 239.255.255.250:1900 //多播地址
	St: ssdp:all //搜索所有 也可以指定特殊类型设备

### UPnP响应

	HTTP/1.1 200 OK
	CACHE-CONTROL: max-age=100 //生命周期
	DATE: Tue, 06 Sep 2016 08:59:17 GMT //响应时间
	EXT: //想控制点确认MAN头域已经被设备理解
	LOCATION: http://192.168.31.242:49153/description.xml //描述文件地址
	OPT: "http://schemas.upnp.org/upnp/1/0/"; ns=01
	01-NLS: 36b309d6-1dd2-11b2-a747-e67ed51c3f33
	SERVER: Linux/2.6.32.11.as, UPnP/1.0, Portable SDK for UPnP devices/1.6.18 //版本信息
	X-User-Agent: redsonic
	ST: upnp:rootdevice
	USN: uuid:a22d1223-d889-b76b-0cc3-4c484a00002e::upnp:rootdevice

### UPnP通知

	NOTIFY * HTTP/1.1
	HOST: 239.255.255.250:1900 //多播地址
	CACHE-CONTROL: max-age=100 //缓存时间
	LOCATION: http://192.168.31.242:49153/description.xml //描述文件地址
	OPT: "http://schemas.upnp.org/upnp/1/0/"; ns=01 
	01-NLS: 36b309d6-1dd2-11b2-a747-e67ed51c3f33
	NT: urn:schemas-upnp-org:service:ConnectionManager:1
	NTS: ssdp:alive
	SERVER: Linux/2.6.32.11.as, UPnP/1.0, Portable SDK for UPnP devices/1.6.18
	X-User-Agent: redsonic
	USN: uuid:a22d1223-d889-b76b-0cc3-4c484a00002e::urn:schemas-upnp-org:service:ConnectionManager:1

NT GENA 规定使用的标头。通知类型。必须采用以下一种形式。单一 URI。

* UPnP:rootdevice
向根设备发送一次。

* uuid:device-UUID
向每种设备 （根设备或嵌入式设备） 发送一次。 设备 UUID 由 UPnP
厂商指定。

* urn：schemas-UPnP-org:device:deviceType:v
向每种设备（根设备或嵌入式设备）发送一次。设备类型与版本
由 UPnP 论坛工作委员会定义。

* urn：schemas-UPnP-org:service:serviceType:v
向每种服务发送一次。服务类型与版本由 UPnP 论坛工作委员会
定义。

NTS GENA 规定使用的标头。通知子类型。必须是 ssdp:alive。单一 URI。

USN SSDP 要求使用的标头。 唯一服务名称。 必须是以下一种。
前缀（位于双冒号前）必须与设备描述中的 UDN 元素值相匹配。单一 URI。

* uuid:device-UUID::UPnP:rootdevice
向根设备发送一次。设备 UUID 由 UPnP 厂商指定。

* uuid:device-UUID
向每种设备 （根设备或嵌入式设备） 发送一次。 设备 UUID 由 UPnP
厂商指定。

* uuid:device-UUID::urn:schemas-UPnP-org:device:deviceType:v
向每种设备 （根设备或嵌入式设备） 发送一次。 设备 UUID 由 UPnP
厂商指定。设备类型与版本由 UPnP 论坛工作委员会定义。

* uuid:device-UUID::urn:schemas-UPnP-org:service:serviceType:v
向每种服务发送一次。设备 UUID 由 UPnP 厂商指定。服务类型
与版本由 UPnP 论坛工作委员会定义。

### UPnP离线

	NOTIFY * HTTP/1.1
	HOST: 239.255.255.250:1900 //多播地址
	NT: urn:schemas-upnp-org:service:ConnectionManager:1
	NTS: ssdp:byebye
	SERVER: Linux/2.6.32.11.as, UPnP/1.0, Portable SDK for UPnP devices/1.6.18
	X-User-Agent: redsonic
	USN: uuid:a22d1223-d889-b76b-0cc3-4c484a00002e::urn:schemas-upnp-org:service:ConnectionManager:1

## UPnP设备描述

一个设备的 UPnP 描述包含多个特定厂商信息、所有嵌入式设备定义、设
备展示 URL、以及所有服务列表，包括控制 URL 和事件触发 URL。除了定义非
标准设备之外，UPnP 厂商可以为标准设备添加新的嵌入式设备和服务。

	<?xml version="1.0"?>
	<root xmlns="urn:schemas-upnp-org:device-1-0">
		<specVersion>
			<major>1</major>
			<minor>0</minor>
		</specVersion>
		<URLBase>base URL for all relative URLs</URLBase>
		<device>
			<deviceType>urn:schemas-upnp-org:device:deviceType:v</deviceType>
			<friendlyName>short user-friendly title</friendlyName>
			<manufacturer>manufacturer name</manufacturer>
			<manufacturerURL>URL to manufacturer site</manufacturerURL>
			<modelDescription>long user-friendly title</modelDescription>
			<modelName>model name</modelName>
			<modelNumber>model number</modelNumber>
			<modelURL>URL to model site</modelURL>
			<serialNumber>manufacturer's serial number</serialNumber>
			<UDN>uuid:UUID</UDN>
			<UPC>Universal Product Code</UPC>
			<iconList>
				<icon>
				<mimetype>image/format</mimetype>
				<width>horizontal pixels</width>
				<height>vertical pixels</height>
				<depth>color depth</depth>
				<url>URL to icon</url>
				</icon>
				XML to declare other icons, if any, go here
			</iconList>
			<serviceList>
				<service>
				<serviceType>urn:schemas-upnp-org:service:serviceType:v</serviceType>
				<serviceId>urn:upnp-org:serviceId:serviceID</serviceId>
				<SCPDURL>URL to service description</SCPDURL>
				<controlURL>URL for control</controlURL>
				<eventSubURL>URL for eventing</eventSubURL>
				</service>
				Declarations for other services defined by a UPnP Forum working committee (if any)
				go here
				Declarations for other services added by UPnP vendor (if any) go here
			</serviceList>
			<deviceList>
				Description of embedded devices defined by a UPnP Forum working committee (if any)
				go here
				Description of embedded devices added by UPnP vendor (if any) go here
			</deviceList>
			<presentationURL>URL for presentation</presentationURL>
		</device>
	</root>


## UPnP控制描述

该描述依据设备中的服务不同而不同，在此略过。

## DLNA 代码实现

### 应用介绍

目前来说在android中用到的UPNP框架基本为cyberlink框架和cling框架，本文将对cling框架进行剖析。因为UPnP已经为音视频领域定义了几个服务，因此我们只要拿来用就可以，如下几个重要服务：

* AVTransport：传输服务，提供媒体文件传输，播放控制等功能。
* ContentDirectory：内容目录，用于提供媒体文件浏览，检索，获取媒体文件信息等功能。
* ConnectionManager：连接管理，用于提供连接方面的管理，例如获取源/目的双方支持的MIME格式信息。
* RendringControl：渲染控制，用于播放时的一些渲染控制，如调节音量，调节亮度等。厂商也可自定义服务

### cling框架

![img][3]

接下来展示几个主要的接口

1. UpnpService 接口

    	public interface UpnpService {

	    	public UpnpServiceConfiguration getConfiguration();//获取为UPnP提供服务的其它配置资源
	    	public ControlPoint getControlPoint();//获取控制点
	    	public ProtocolFactory getProtocolFactory();//获取UPnP协议栈的协议工厂
	    	public Registry getRegistry();//获取本地注册表
	    	public Router getRouter();//获取网络路由对象
	    	public void shutdown();//关闭

    	}
	
2. Router 接口

		public interface Router {
		
		    public UpnpServiceConfiguration getConfiguration();
		    public ProtocolFactory getProtocolFactory();
		    public NetworkAddressFactory getNetworkAddressFactory();
		    public List<NetworkAddress> getActiveStreamServers(InetAddress preferredAddress);
		    public void shutdown();
		    public void received(IncomingDatagramMessage msg);
		    public void received(UpnpStream stream);
		    public void send(OutgoingDatagramMessage msg);
		    public StreamResponseMessage send(StreamRequestMessage msg);
		    public void broadcast(byte[] bytes);
		}

3. Registry 接口
	
		public interface Registry {
			.
			.
			.
			public Device getDevice(UDN udn, boolean rootOnly);
		    public LocalDevice getLocalDevice(UDN udn, boolean rootOnly);
		    public RemoteDevice getRemoteDevice(UDN udn, boolean rootOnly);
		    public Collection<LocalDevice> getLocalDevices();
		    public Collection<RemoteDevice> getRemoteDevices();
		    public Collection<Device> getDevices();
		    public Collection<Device> getDevices(DeviceType deviceType);
		    public Collection<Device> getDevices(ServiceType serviceType);
		    public Service getService(ServiceReference serviceReference);
			.
			.
			.
		
		}

4. ControlPoint 接口

		public interface ControlPoint {
		
		    public UpnpServiceConfiguration getConfiguration();
		    public ProtocolFactory getProtocolFactory();
		    public Registry getRegistry();
		    public void search();
		    public void search(UpnpHeader searchType);
		    public void search(int mxSeconds);
		    public void search(UpnpHeader searchType, int mxSeconds);
		    public void execute(ActionCallback callback);
		    public void execute(SubscriptionCallback callback);
		
		}

5. ProtocolFactory 接口
	
		public interface ProtocolFactory {
		
		    public UpnpService getUpnpService();
		    public ReceivingAsync createReceivingAsync(IncomingDatagramMessage message) throws ProtocolCreationException;
		    public ReceivingSync createReceivingSync(StreamRequestMessage requestMessage) throws ProtocolCreationException;
		    public SendingNotificationAlive createSendingNotificationAlive(LocalDevice localDevice);
		    public SendingNotificationByebye createSendingNotificationByebye(LocalDevice localDevice);
		    public SendingSearch createSendingSearch(UpnpHeader searchTarget, int mxSeconds);
		    public SendingAction createSendingAction(ActionInvocation actionInvocation, URL controlURL);
		    public SendingSubscribe createSendingSubscribe(RemoteGENASubscription subscription);
		    public SendingRenewal createSendingRenewal(RemoteGENASubscription subscription);
		    public SendingUnsubscribe createSendingUnsubscribe(RemoteGENASubscription subscription);
		    public SendingEvent createSendingEvent(LocalGENASubscription subscription);
		}

6. UpnpServiceConfiguration 接口
	
		public interface UpnpServiceConfiguration {
		
		    public NetworkAddressFactory createNetworkAddressFactory();
		    public DatagramProcessor getDatagramProcessor();
		    public SOAPActionProcessor getSoapActionProcessor();
		    public GENAEventProcessor getGenaEventProcessor();
		    public StreamClient createStreamClient();
		    public MulticastReceiver createMulticastReceiver(NetworkAddressFactory networkAddressFactory);
		    public DatagramIO createDatagramIO(NetworkAddressFactory networkAddressFactory);
		    public StreamServer createStreamServer(NetworkAddressFactory networkAddressFactory);
		    public Executor getMulticastReceiverExecutor();
		    public Executor getDatagramIOExecutor();
		    public Executor getStreamServerExecutor();
		    public DeviceDescriptorBinder getDeviceDescriptorBinderUDA10();
		    public ServiceDescriptorBinder getServiceDescriptorBinderUDA10();
		    public ServiceType[] getExclusiveServiceTypes();
		    public int getRegistryMaintenanceIntervalMillis();
		    public Executor getAsyncProtocolExecutor();
		    public Executor getSyncProtocolExecutor();
		    public Namespace getNamespace();
		    public Executor getRegistryMaintainerExecutor();
		    public Executor getRegistryListenerExecutor();
		    public void shutdown();
		}


![img][4]


HTTPMU  基于 UDP 的 HTTP 多播

HTTPU  基于 UDP 的 HTTP（单播）