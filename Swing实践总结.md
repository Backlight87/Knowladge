### Swing代码总结：
情景：需要用eclipse写一个gui的界面，不需要太复杂，简单的布局显示和点击事件上传或者是选择文件的功能就okay。

#### 无事件的简单布局：

备注知识：

主要就是实例JFrame，控制JFrame的位置，然后不断往JFrame里一直塞其他控件，
JFrame->JScrollPane ->JPane->Jtutton,Jlist等等，前面的可以装后面的任意一种

JFrame 的默认布局是（BorderLayout，东西南北中）

JPane的默认布局是（FlowLayout，一个接一个排，排不下了换行/换列）

还有无论什么布局，看完后记得多看几遍你的每个控件所在位置，很多bug你会发现是控件在位置上相互遮挡甚至是相互覆盖造成的

以及如果你设置了JScrollPane一层，记得也要给JScrollPane设置setBounds，不然会看不到滚动条

JLabel 不仅可以放文字也可以放图片，有时候在太长了放不下怎么办，可以尝试用HTML语法“只要嵌入<html></html>”他就会自动换行，但是对于数字你必须还要嵌入<br>才可以换行。


JFrame->JScrollPane ->JPane 这三个是容器，你对容器里的容器或者控件设置setBounds（x位置，y位置，宽，高），他的xy其实是相对父容器的



```
JFrame jframe = new JFrame(title);// 实例化一个JFrame，“title”是最后显示框的显示
		JPanel apk1_JPane_UpLoad = new JPanel(); // 选择第一个按钮的jpanel
		JPanel apk2_JPane_UpLoad = new JPanel(); // 选择第二个按钮的jpanel
		JPanel apk_JPane_Show = new JPanel();// 展示数据的时候的jpanel
		jframe.setLayout(null);//把布局置空后面才能用绝对布局s
		jframe.add(apk1_JPane_UpLoad);
		jframe.add(apk2_JPane_UpLoad);

    // 添加滚动条
		JScrollPane apk_JScrollPane_Show = new JScrollPane(apk_JPane_Show);
		jframe.add(apk_JScrollPane_Show);

		apk1_JPane_UpLoad.setBounds(200, 10, 400, 80);
		apk2_JPane_UpLoad.setBounds(630, 10, 400, 80);
		apk2_JPane_UpLoad.setLayout(null);
		apk1_JPane_UpLoad.setLayout(null);
		apk_JPane_Show.setLayout(null);
		apk_JPane_Show.setBounds(10, 100, 1180, 850);
		apk_JPane_Show.setPreferredSize(new Dimension(1180, 850));//对于jpanel记得设置setPreferredSize，不然显示不出滚动条，这点还没搞懂
		apk_JScrollPane_Show.setBounds(10, 100, 1180, 850);

//=======================下面的init都是把面板传进去，然后设置一些子控件===========
		initVersion(apk_JPane_Show);
		initSize(apk_JPane_Show);
		initPackageName(apk_JPane_Show);
		initPermission(apk_JPane_Show);
		initApk1Select(apk1_JPane_UpLoad);
		initApk2Select(apk2_JPane_UpLoad);


		jframe.setVisible(true);// 可见
		jframe.setSize(1200, 990);// 窗体大小
		jframe.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);// close的方式

    //=====下面是为了让屏幕显示在正中间========================
		int windowWidth = jframe.getWidth(); // 获得窗口宽
		int windowHeight = jframe.getHeight(); // 获得窗口高
		Toolkit kit = Toolkit.getDefaultToolkit(); // 定义工具包
		Dimension screenSize = kit.getScreenSize(); // 获取屏幕的尺寸
		int screenWidth = screenSize.width; // 获取屏幕的宽
		int screenHeight = screenSize.height; // 获取屏幕的高
		jframe.setLocation(screenWidth / 2 - windowWidth / 2, screenHeight / 2 - windowHeight / 2);

    jframe.setResizable(false);//设置窗口不允许用户去拉大拉小


```


#### 点击时间文件上传或者是选择

首先JButton注册点击事件

```

//apk1_JButton_UpLoad是一个JButton按钮
apk1_JButton_UpLoad.addMouseListener(new MouseAdapter() { // 添加鼠标点击事件
			public void mouseClicked(MouseEvent event) {
				eventOnImport(new JButton(), FLAG_APK1);//在点击事件里随意添加自己的逻辑
		}); // 文件上传功能


```

具体的上传逻辑，如果我们不想要上传只想要选择，也可以，在得到fileName的地方其实我们已经get到这个file了，我们可以用file的getAbsolutePath()方法，就可以得到路径了，我们可以在任何需要的地方去用inputstream去读取这个File文件，所以我们可以删掉后面的输入输出部分的代码。


```

/**
	 * 文件上传功能
	 *
	 * @param apk_JButton_UpLoad
	 *            按钮控件名称
	 */
	public static void eventOnImport(JButton apk_JButton_UpLoad, String flag) {
		JFileChooser chooser = new JFileChooser();//文件选择器
		chooser.setMultiSelectionEnabled(true);
		/** 过滤文件类型 * */
		FileNameExtensionFilter filter = new FileNameExtensionFilter("war", "xml", "txt", "doc", "docx", "zip", "apk");
		chooser.setFileFilter(filter);
		int returnVal = chooser.showOpenDialog(apk_JButton_UpLoad);
		if (returnVal == JFileChooser.APPROVE_OPTION) {
			/** 得到选择的文件* */
			File[] arrfiles = chooser.getSelectedFiles();
			if (arrfiles == null || arrfiles.length == 0) {
				return;
			}
			FileInputStream input = null;
			FileOutputStream out = null;
			String path = "./";//设置存储的位置是当前项目的上一层
			String fileName = "";
			try {
				for (File f : arrfiles) {
					File dir = new File(path);
					fileName = f.getName();//这里得到的是文件的名字
					/** 目标文件夹 * */
					File[] fs = dir.listFiles();
					HashSet<String> set = new HashSet<String>();
					for (File file : fs) {
						set.add(file.getName());//这里得到的不是

					}
					/** 判断是否已有该文件* */
					if (set.contains(f.getName())) {
						JOptionPane.showMessageDialog(new JDialog(), f.getName() + ":该文件已存在！");
						return;
					}
					input = new FileInputStream(f);
					byte[] buffer = new byte[1024];
					File des = new File(path, f.getName());
					out = new FileOutputStream(des);
					int len = 0;
					while (-1 != (len = input.read(buffer))) {
						out.write(buffer, 0, len);
					}

					out.close();
					input.close();
				}
				JOptionPane.showMessageDialog(null, "上传成功！", "提示", JOptionPane.INFORMATION_MESSAGE);
			} catch (FileNotFoundException e1) {
				JOptionPane.showMessageDialog(null, "上传失败！", "提示", JOptionPane.ERROR_MESSAGE);
				e1.printStackTrace();
			} catch (IOException e1) {
				JOptionPane.showMessageDialog(null, "上传失败！", "提示", JOptionPane.ERROR_MESSAGE);
				e1.printStackTrace();
			}
		}
	}


```

附录1：完整代码及界面：

```
public class ComparedApkFace {

	private final static String FLAG_APK1 = "first";// 如果传进去的是Second就代表要对apk2_File_Address赋值
	private final static String FLAG_APK2 = "second";// 如果传进去的是Second就代表要对apk2_File_Address赋值
	private static String apk1_File_Name;// 上传按钮1上传的apk名字
	private static String apk2_File_Name;// 上传按钮2上传的apk名字
	private static String apk1_File_Address = "";// 上传按钮1上传的apk地址
	private static String apk2_File_Address = "";// 上传按钮2上传的apk地址
	private static JLabel apk1_JLabel_Address;// 上传按钮后把apk地址显示在这里
	private static JLabel apk2_JLabel_Address;
	private JLabel apk1_JLabel_Version;
	private JLabel apk2_JLabel_Version;
	private DefaultListModel<String> apk_listModel_Permission;// Jlist显示的数据源
	private DefaultListModel<String> apk1_listModel_Permission;
	private DefaultListModel<String> apk2_listModel_Permission;
	private JLabel apk_JLabel_Permission;
	private JLabel apk1_JLabel_Permission;
	private JLabel apk2_JLabel_Permission;
	private JLabel apk1_JLabel_Size;
	private JLabel apk2_JLabel_Size;
	private JLabel apk_JLabel_Size;
	private JLabel apk1_JLabel_PackageName;
	private JLabel apk2_JLabel_PackageName;
	// 给三个权限Jlist设置滚动框
	private JScrollPane apk1_JScrollPane_Permission;
	private JScrollPane apk_JScrollPane_Permission;
	private JScrollPane apk2_JScrollPane_Permission;

	public void comparedApkFace(String title) {
		JFrame jframe = new JFrame(title);// 实例化一个JFrame
		JPanel apk1_JPane_UpLoad = new JPanel(); // 选择第一个按钮的jpanel
		JPanel apk2_JPane_UpLoad = new JPanel(); // 选择第一个按钮的jpanel
		JPanel apk_JPane_Show = new JPanel();// 展示数据的时候的jpanel
		jframe.setLayout(null);

		jframe.add(apk1_JPane_UpLoad);
		jframe.add(apk2_JPane_UpLoad);
		// 添加滚动条
		JScrollPane apk_JScrollPane_Show = new JScrollPane(apk_JPane_Show);
		jframe.add(apk_JScrollPane_Show);

		apk1_JPane_UpLoad.setBounds(200, 10, 400, 80);
		apk2_JPane_UpLoad.setBounds(630, 10, 400, 80);
		apk2_JPane_UpLoad.setLayout(null);
		apk1_JPane_UpLoad.setLayout(null);
		apk_JPane_Show.setLayout(null);
		apk_JPane_Show.setBounds(10, 100, 1180, 850);
		apk_JPane_Show.setPreferredSize(new Dimension(1180, 850));
		apk_JScrollPane_Show.setBounds(10, 100, 1180, 850);

		initVersion(apk_JPane_Show);
		initSize(apk_JPane_Show);
		initPackageName(apk_JPane_Show);
		initPermission(apk_JPane_Show);
		initApk1Select(apk1_JPane_UpLoad);
		initApk2Select(apk2_JPane_UpLoad);

		jframe.setVisible(true);// 可见
		jframe.setSize(1200, 990);// 窗体大小
		jframe.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);// close的方式
		int windowWidth = jframe.getWidth(); // 获得窗口宽
		int windowHeight = jframe.getHeight(); // 获得窗口高
		Toolkit kit = Toolkit.getDefaultToolkit(); // 定义工具包
		Dimension screenSize = kit.getScreenSize(); // 获取屏幕的尺寸
		int screenWidth = screenSize.width; // 获取屏幕的宽
		int screenHeight = screenSize.height; // 获取屏幕的高
		jframe.setLocation(screenWidth / 2 - windowWidth / 2, screenHeight / 2 - windowHeight / 2);
		jframe.setResizable(false);
	}

	/*
	 * 初始化上傳選擇第一个apk的控件以及点击事件
	 *
	 */
	public void initApk1Select(JPanel jPanel) {
		JButton apk1_JButton_UpLoad = new JButton("上传APK1");
		apk1_JButton_UpLoad.setHorizontalAlignment(SwingConstants.CENTER);
		apk1_JLabel_Address = new JLabel("");// 创建一个Label标签
		apk1_JButton_UpLoad.setBounds(0, 20, 100, 40);
		apk1_JLabel_Address.setBounds(120, 20, 280, 40);
		jPanel.add(apk1_JButton_UpLoad);
		jPanel.add(apk1_JLabel_Address);// 将标签添加到容器中
		apk1_JButton_UpLoad.addMouseListener(new MouseAdapter() { // 添加鼠标点击事件
			public void mouseClicked(MouseEvent event) {
				eventOnImport(new JButton(), FLAG_APK1);
				if (apk1_File_Address.toString() != "" && apk2_File_Address.toString() != "") {// 如果两个字符串都不为空证明已经上传了两个apk了，可以去解析了
					if (!apk2_File_Address.equals(apk1_File_Address)) {
						try {
							showInfo();
						} catch (Exception e) {
						}
					} else {
						JOptionPane.showMessageDialog(new JDialog(), "Warn!APK1和APK2不能是同一个APK!!!");
					}
				}
			}
		}); // 文件上传功能
	}

	/*
	 * 初始化上傳選擇第二个apk的控件以及点击事件
	 *
	 */
	public void initApk2Select(JPanel jPanel) {

		apk2_JLabel_Address = new JLabel("");// 创建一个Label标签
		apk2_JLabel_Address.setHorizontalAlignment(SwingConstants.LEFT);
		JButton apk2_JButton_UpLoad = new JButton("上传APK2");
		apk2_JButton_UpLoad.setHorizontalAlignment(SwingConstants.CENTER);
		apk2_JButton_UpLoad.setBounds(0, 20, 100, 40);
		apk2_JLabel_Address.setBounds(120, 20, 280, 40);
		jPanel.add(apk2_JButton_UpLoad);
		jPanel.add(apk2_JLabel_Address);// 将标签添加到容器中
		apk2_JButton_UpLoad.addMouseListener(new MouseAdapter() { // 添加鼠标点击事件
			public void mouseClicked(MouseEvent event) {
				eventOnImport(new JButton(), FLAG_APK2);
				if (apk1_File_Address.toString() != "" && apk2_File_Address.toString() != "") {// 如果两个字符串都不为空证明已经上传了两个apk了，可以去解析了
					if (!apk2_File_Address.equals(apk1_File_Address)) {
						try {
							showInfo();
						} catch (Exception e) {
						}
					} else {
						JOptionPane.showMessageDialog(new JDialog(), "Warn!APK1和APK2不能是同一个APK!!!");
					}
				}
			}
		});

	}

	/*
	 *
	 * 初始化size相關的控件
	 */
	public void initSize(JPanel jPanel) {
		JLabel apk_JLabel_Size_Title = new JLabel("包大小 ： ");
		apk_JLabel_Size_Title.setBounds(50, 70, 140, 30);
		jPanel.add(apk_JLabel_Size_Title);

		apk1_JLabel_Size = new JLabel("");
		apk1_JLabel_Size.setBounds(200, 70, 140, 30);
		jPanel.add(apk1_JLabel_Size);

		apk2_JLabel_Size = new JLabel(" ");
		apk2_JLabel_Size.setBounds(420, 70, 140, 30);
		jPanel.add(apk2_JLabel_Size);

		apk_JLabel_Size = new JLabel("");
		apk_JLabel_Size.setBounds(630, 70, 140, 30);
		jPanel.add(apk_JLabel_Size);

	}

	/*
	 * 初始化权限相关的控件
	 *
	 */
	public void initPermission(JPanel jPanel) {

		JLabel apk1_JLabel_Permission_Title = new JLabel("权限  ：  ");
		apk1_JLabel_Permission_Title.setBounds(50, 170, 180, 30);
		jPanel.add(apk1_JLabel_Permission_Title);

		apk1_JLabel_Permission = new JLabel(" ");
		apk1_JLabel_Permission.setBounds(200, 170, 180, 30);
		jPanel.add(apk1_JLabel_Permission);

		apk1_listModel_Permission = new DefaultListModel<>();
		JList<String> apk1_JList_Permission_Value = new JList<>(apk1_listModel_Permission);
		apk1_JList_Permission_Value.setBounds(200, 210, 400, 180);

		apk1_JScrollPane_Permission = new JScrollPane(apk1_JList_Permission_Value);
		apk1_JList_Permission_Value.setVisibleRowCount(4);
		apk1_JScrollPane_Permission.setBounds(200, 210, 400, 180);
		jPanel.add((apk1_JScrollPane_Permission));

		apk2_JLabel_Permission = new JLabel(" ");
		apk2_JLabel_Permission.setBounds(200, 390, 180, 30);
		jPanel.add(apk2_JLabel_Permission);

		apk2_listModel_Permission = new DefaultListModel<>();
		JList<String> apk2_JList_Permission_Value = new JList<>(apk2_listModel_Permission);
		apk2_JList_Permission_Value.setBounds(200, 430, 400, 180);
		apk2_JScrollPane_Permission = new JScrollPane(apk2_JList_Permission_Value);
		apk2_JList_Permission_Value.setVisibleRowCount(4);
		apk2_JScrollPane_Permission.setBounds(200, 430, 400, 180);
		jPanel.add(apk2_JScrollPane_Permission);

		apk_JLabel_Permission = new JLabel("");
		apk_JLabel_Permission.setBounds(200, 610, 180, 30);
		jPanel.add(apk_JLabel_Permission);

		apk_listModel_Permission = new DefaultListModel<>();
		JList<String> apk_JList_Permission_Value = new JList<>(apk_listModel_Permission);
		apk_JList_Permission_Value.setBounds(200, 640, 400, 180);

		apk_JScrollPane_Permission = new JScrollPane(apk_JList_Permission_Value);
		apk_JList_Permission_Value.setVisibleRowCount(4);
		apk_JScrollPane_Permission.setBounds(200, 640, 400, 180);
		jPanel.add(apk_JScrollPane_Permission);

		apk_JScrollPane_Permission.setVisible(false);
		apk2_JScrollPane_Permission.setVisible(false);
		apk1_JScrollPane_Permission.setVisible(false);

	}

	/*
	 * 初始化包名相关的控件
	 *
	 */
	public void initPackageName(JPanel jPanel) {

		JLabel apk_JLabel_Size_PackageName_Title = new JLabel("包名  ： ");
		apk_JLabel_Size_PackageName_Title.setBounds(50, 120, 140, 40);
		jPanel.add(apk_JLabel_Size_PackageName_Title);

		apk1_JLabel_PackageName = new JLabel("");
		apk1_JLabel_PackageName.setBounds(200, 120, 400, 40);
		jPanel.add(apk1_JLabel_PackageName);

		apk2_JLabel_PackageName = new JLabel("");
		apk2_JLabel_PackageName.setBounds(630, 120, 400, 40);
		jPanel.add(apk2_JLabel_PackageName);

	}

	/*
	 * 初始化version相关的控件
	 */
	public void initVersion(JPanel jPanelVersion) {

		JLabel apk_JLabel_Version_Title = new JLabel("版本号 ： ");
		apk_JLabel_Version_Title.setBounds(50, 10, 140, 30);
		jPanelVersion.add(apk_JLabel_Version_Title);

		apk1_JLabel_Version = new JLabel(" ");
		apk1_JLabel_Version.setBounds(200, 10, 140, 30);
		jPanelVersion.add(apk1_JLabel_Version);

		apk2_JLabel_Version = new JLabel(" ");
		apk2_JLabel_Version.setBounds(420, 10, 140, 30);
		jPanelVersion.add(apk2_JLabel_Version);

	}

	/**
	 * 点击按钮选择对应的APK，选择后显示选择的文件的地址
	 *
	 * @param apk_JButton_UpLoad,flag
	 *            上传按钮和对应的 flag 按钮控件名称
	 */
	public static void eventOnImport(JButton apk_JButton_UpLoad, String flag) {
		JFileChooser chooser = new JFileChooser();
		chooser.setMultiSelectionEnabled(true);
		/** 过滤文件类型 * */
		FileNameExtensionFilter filter = new FileNameExtensionFilter("war", "xml", "txt", "doc", "docx", "zip", "apk");
		chooser.setFileFilter(filter);
		int returnVal = chooser.showOpenDialog(apk_JButton_UpLoad);
		if (returnVal == JFileChooser.APPROVE_OPTION) {
			/** 得到选择的文件* */
			File[] arrfiles = chooser.getSelectedFiles();
			if (arrfiles == null || arrfiles.length == 0) {
				return;
			}
			try {
				for (File f : arrfiles) {
					if (flag.equals(FLAG_APK1)) {
						apk1_File_Name = f.getName();
						apk1_File_Address = f.getAbsolutePath();

						apk1_JLabel_Address.setText("<html>" + apk1_File_Name + "</html>");

					} else if (flag.equals(FLAG_APK2)) {
						apk2_File_Name = f.getName();
						apk2_File_Address = f.getAbsolutePath();
						apk2_JLabel_Address.setText("<html>" + apk2_File_Name + "</html>");
					}
				}

			} catch (Exception e) {

				return;

			}
		}
	}

	/**
	 * 解析APK,显示解析的信息到界面上
	 *
	 */
	public void showInfo() {

		ComparedApk comparedApk = new ComparedApk();

		try {

			// 获取数据
			ComparedApk.initApkReader(apk1_File_Address, apk2_File_Address);
			comparedApk.getSharePermission(apk_listModel_Permission);
			comparedApk.getOnlyPermission1(apk1_listModel_Permission);
			comparedApk.getOnlyPermission2(apk2_listModel_Permission);

			// 获取数据后填充到界面上
			apk1_JScrollPane_Permission.setVisible(true);
			apk_JScrollPane_Permission.setVisible(true);
			apk2_JScrollPane_Permission.setVisible(true);

			apk_JLabel_Permission.setText("两个apk共同拥有的权限：  ");
			apk1_JLabel_Permission.setText("APK1独自拥有的权限是 ： ");
			apk2_JLabel_Permission.setText("APK2独自拥有的权限是 ： ");

			apk1_JLabel_Version.setText("APK1  =  " + comparedApk.getVersionCode1());
			apk2_JLabel_Version.setText("APK2  =  " + comparedApk.getVersionCode2());

			apk1_JLabel_Size.setText("APK1  =  " + comparedApk.getApk1Size() + "  M");
			apk2_JLabel_Size.setText("APK2  =  " + comparedApk.getApk2Size() + "  M");

			apk_JLabel_Size.setText("包相差 " + "  =  " +comparedApk.getApkSizeDif()  + "  M");

			apk1_JLabel_PackageName.setText("<html>APK1 = " + comparedApk.getPackageName1() + "</html>");
			apk2_JLabel_PackageName.setText("<html>APK2 = " + comparedApk.getPackageName2() + "</html>");

		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}

	}

```


实现界面：

![实现的界面](http://oi2e3199v.bkt.clouddn.com/8a032f7920f526824ed851b3a2a24208.png?imageView2/2/w/700/h/500)





附录2 :一些概念解释和教程
```

JFrame – java的GUI程序的基本思路是以JFrame为基础，它是屏幕上window的对象，能够最大化、最小化、关闭。

JPanel – Java图形用户界面(GUI)工具包swing中的面板容器类，包含在javax.swing 包中，可以进行嵌套，功能是对窗体中具有相同逻辑功能的组件进行组合，是一种轻量级容器，可以加入到JFrame窗体中。。
JLabel – JLabel 对象可以显示文本、图像或同时显示二者。可以通过设置垂直和水平对齐方式，指定标签显示区中标签内容在何处对齐。默认情况下，标签在其显示区内垂直居中对齐。默认情况下，只显示文本的标签是开始边对齐；而只显示图像的标签则水平居中对齐。
JTextField –一个轻量级组件，它允许编辑单行文本。
JPasswordField – 允许我们输入了一行字像输入框，但隐藏星号(*) 或点创建密码(密码)
JButton – JButton 类的实例。用于创建按钮类似实例中的 "Login"。
JButton - 跟android的list一样可以用来展示条目
JScrollPane - 滚动框
```

Jlist的用法：

![三种显示格式](http://oi2e3199v.bkt.clouddn.com/1dea49681b5c92d6b171187176765b65.png?imageView2/2/w/700/h/500)

分别为HORIZONTAL_WRAP、VERTICAL_WRAP、VERTICAL，通过setLayoutOrientation 进行设置。
```




        private JList list;//初始化jlist
        private DefaultListModel listModel;//初始化list显示的数据源，推荐用这个，如果是用默认的数据源，我们就无法对list里的数据进行动态的增删改，调用model的add/remove/insert等方法来实现元素的添加和删除。
        listModel = new DefaultListModel();
        listModel.addElement("Jane Doe");
        listModel.addElement("John Smith");
        listModel.addElement("Kathy Green");

        list = new JList(listModel);//填充数据源，数据变了，展示的list会自动变
        list.setSelectionMode(ListSelectionModel.SINGLE_SELECTION);
        list.setVisibleRowCount(5);//设置显示的行数是5行
        JScrollPane listScrollPane = new JScrollPane(list);//设置滚动

        Jlist还有数据变换事件，事件选择事件等等，需要用的时候自己去搜一下就有
```

附录3 ：一些常用的设置

```
1、设置窗口的图标
默认的Jframe左上角的图标时Java的咖啡杯图标，以下代码用来自定义图标：

String logoFilePath = "/icon/a.png";//图标目录
JFrame jFrame=new JFrame(frameTitle);
ImageIcon imageIcon = new ImageIcon(this.getClass().getResource(logoFilePath));
java.awt.Image image = imageIcon.getImage();
jFrame.setIconImage(image);
 图标最好不要弄成ico格式的，我测试过好几次，每次都像没有设置一样，jpeg、jpg、png格式最好。

2、窗口带有滚动条
swing里面有个滚动条的控件，叫JScrollPane，把显示的控件放到这个控件里面，当数据过多时会自动出现滚动条，如下面的代码：

JScrollPane scrl = new JScrollPane(jPanel,JScrollPane.VERTICAL_SCROLLBAR_ALWAYS,JScrollPane.HORIZONTAL_SCROLLBAR_NEVER);
在将JScrollPane放到Frame中：

JFrame jFrame=new JFrame(frameTitle);
Container cont = jFrame.getContentPane();
//添加到带有滚动条的panel
JScrollPane scrl = new JScrollPane(jPanel,JScrollPane.VERTICAL_SCROLLBAR_ALWAYS,JScrollPane.HORIZONTAL_SCROLLBAR_NEVER);
cont.add(scrl);
jFrame.setVisible(true);//可见
第一个参数：需要放到JScrollPane控件中的内容，我这里是一个Jpanel，所有的东西都放在这个Jpanel上，然后将JPanel放在JScrollPane里面。

第二个参数：设置垂直滚动条的显示方式，有五种方式
VERTICAL_SCROLLBAR_ALWAYS：一直显示
VERTICAL_SCROLLBAR_AS_NEEDED：需要的时候显示，类似于自动
VERTICAL_SCROLLBAR_NEVER：从来不显示
VERTICAL_SCROLLBAR
VERTICAL_SCROLLBAR_POLICY
第三个参数：设置水平滚动条的显示方式，也有五种，类型垂直滚动条的方式。
以上设置完了之后，滚动条会出来，但是没有滑块，也就是不能上下滑动，还需要设置一下放在里面的panel的宽和高。如下：

//设置大小（必须设置，否则不会出现滚动条滑块）
jPanel.setPreferredSize(new Dimension(530, 1070));
 第一个参数：宽度，当放在JScrollPane中的内容的宽大超过该宽度且设置为允许横向滚动条出现时就会出现横向滚动条，否则不出现；
第二参数：高度，出现条件同横向滚动条

3、设置背景色
这个比较简单：jPanel.setBackground(new Color(r,g,b));但是我们通常拿到的都是16进制的数据，类似于5a9bd5这种的，而设置背景的时候需要的是rgb三原色的数值，因此需要转化一下。

/**
 * Color对象转换成字符串
 * @param color Color对象
 * @return 16进制颜色字符串
 * */
public static String toHexFromColor(Color color){
    String r,g,b;
    StringBuilder su = new StringBuilder();
    r = Integer.toHexString(color.getRed());
    g = Integer.toHexString(color.getGreen());
    b = Integer.toHexString(color.getBlue());
    r = r.length() == 1 ? "0" + r : r;
    g = g.length() ==1 ? "0" +g : g;
    b = b.length() == 1 ? "0" + b : b;
    r = r.toUpperCase();
    g = g.toUpperCase();
    b = b.toUpperCase();
    su.append("0xFF");
    su.append(r);
    su.append(g);
    su.append(b);
    //0xFF0000FF
    return su.toString();
}
/**
 * 字符串转换成Color对象
 * @param colorStr 16进制颜色字符串
 * @return Color对象
 * */
public static Color toColorFromString(String colorStr){
    if(colorStr.toUpperCase().startsWith("0XFF")){//说明有16进制的前缀
        colorStr = colorStr.substring(4);
    }
    Color color =  new Color(Integer.parseInt(colorStr, 16)) ;
    return color;
}

4、设置JLabel的背景色、字体颜色

JLabel titleLabel = new JLabel("测试");
//设置组件JLabel不透明，只有设置为不透明，设置背景色才有效
titleLabel.setOpaque(true);
titleLabel.setBackground(Color.red);//设置背景色
titleLabel.setForeground(Color.white);//用前景色属性设置字体颜色
titleLabel.setFont(new Font("宋体",Font.BOLD,16));//字体、加粗、16号大小
若是设置前景色、背景色，上面代码中的那句titleLabel.setOpaque(true); 一定要加，否则看不到效果。

5、JLabel、JTextField的居中
JLabel的居中比较简单，用构造方法就OK：
JLabel testLabel= new JLabel("测试",JLabel.CENTER);//设置名称且居中

文本框的居中则是：
JTextField test= new JTextField("");
test.setHorizontalAlignment(JTextField.CENTER);//居中
对齐类型有：
JTextField.LEFT：居左对齐
JTextField.CENTER：居中对齐
JTextField.RIGHT：居右对齐
JTextField.LEADING：前端对齐
JTextField.TRAILING：尾部对齐


```
