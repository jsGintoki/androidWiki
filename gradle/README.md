# gradle_study
gradle 学习

Gradle 基本概念
首先我们学习几个gradle 的脚步语法，掌握了这几个语法，你就能非常简单的用gradle构建打包android项目了。 首先，我们来看下一个最简单android build.gradle。
build.gradle
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
	
buildscript {
       
	 repositories {
            mavenCentral()
        }

        dependencies {
            classpath 'com.android.tools.build:gradle:0.4'
        }
    }
英语的介绍都来自与 gradle官方文档， 主要后边的中文不是翻译，是补充介绍。。
buildscript{}
Configures the build script classpath for this project. 说白了就是设置脚本的运行环境
repositories{}
Returns a handler to create repositories which are used for retrieving dependencies and uploading artifacts produced by the project. 大意就是支持java 依赖库管理（maven/ivy）,用于项目的依赖。这也是gradle 强力的地方。。。
dependencies{}
The dependency handler of this project. The returned dependency handler instance can be used for adding new dependencies. For accessing already declared dependencies, the configurations can be used. 依赖包的定义。支持maven/ivy，远程，本地库，也支持单文件，如果前面定义了repositories{}maven 库，使用maven的依赖（我没接触过ivy。。）的时候只需要按照用类似于com.android.tools.build:gradle:0.4，gradle 就会自动的往远程库下载相应的依赖。

AS中APP所有的配置尽在一个build.gradle文件中，打包的时候也是解析build.gralde文件来打包的，所以搞懂build.gradle文件是至关重要的，结构如下所示


apply plugin用来指定用的是哪个插件，取值有：
		com.android.application：Android APP插件（打包得到的是.apk文件）
		com.android.library：Android库插件（打包得到的是.aar文件）

android用来指定Android打包插件的相关属性，其包含如下节点
		compileSdkVersion(apiLevel)：设置编译时用的Android版本
		buildToolsVersion(buildToolsVersionName)：设置编译时使用的构建工具的版本
		defaultConfig：设置一些默认属性，其可用属性是buildTypes和ProductFlavors之和
		sourceSets：配置相关源文件的位置，当你的项目的目录结构跟默认的有区别但又不想改的时候sourceSets就派上用场了
		aidl     设置aidi的目录
		assets     设置assets资源目录
		compileConfigurationName     The name of the compile configuration for this source set.
		java     Java源代码目录
		jni     JNI代码目录
		jniLibs     已编译好的JNI库目录
		manifest     指定清单文件
		name     The name of this source set.
		packageConfigurationName     The name of the runtime configuration for this source set.
		providedConfigurationName     The name of the compiled-only configuration for this source set.
		renderscript     Renderscript源代码目录
		res     资源目录
		setRoot(path)     根目录
		signingConfigs：配置签名信息
		keyAlias     签名的别名
		keyPassword     密码
		storeFile     签名文件的路径
		storePassword     签名密码
		storeType     类型
		buildTypes：配置构建类型，可打出不同类型的包，默认有debug和release两种，你还可以在增加N种
		applicationIdSuffix     修改applicationId，在默认applicationId的基础上加后缀。在buildType中修改applicationId时只能加后缀，不能完全修改
		debuggable     设置是否生成debug版的APK
		jniDebuggable     设置生成的APK是否支持调试本地代码
		minifyEnabled     设置是否执行混淆
		multiDexEnabled     Whether Multi-Dex is enabled for this variant.
		renderscriptDebuggable     设置生成的APK是否支持调试RenderScript代码
		renderscriptOptimLevel     设置RenderScript优化级别
		signingConfig     设置签名信息
		versionNameSuffix     修改版本名称，在默认版本名称的基础上加后缀。在buildType中修改版本名称时只能加后缀，不能完全修改
		zipAlignEnabled     设置是否对APK包执行ZIP对齐优化
		proguardFile(proguardFile)     添加一个混淆文件
		proguardFiles(proguardFileArray)     添加多个混淆文件
		setProguardFiles(proguardFileIterable)     设置多个混淆文件
		productFlavors：配置不同风格的APP，在buildTypes的基础上还可以让每一个类型的APP拥有不同的风格，所以最终打出的APK的数量就是buildTypes乘以productFlavors
		applicationId     设置应用ID
		multiDexEnabled     Whether Multi-Dex is enabled for this variant.signingConfig     Signing config used by this product flavor.
		testApplicationId     设置测试时的应用ID
		testFunctionalTest     See instrumentation.
		testHandleProfiling     See instrumentation.
		testInstrumentationRunner     Test instrumentation runner class name.
		versionCode     设置版本号
		versionName     设置版本名称
		minSdkVersion(int minSdkVersion)     设置兼容的最小SDK版本
		minSdkVersion(String minSdkVersion)     设置兼容的最小版本
		proguardFile(proguardFile)     添加一个混淆文件
		proguardFiles(proguardFileArray)     添加多个混淆文件
		setProguardFiles(proguardFileIterable)     设置多个混淆文件
		targetSdkVersion(int targetSdkVersion)     设置目标SDK版本
		targetSdkVersion(String targetSdkVersion)     设置目标SDK版本
		testOptions：设置测试相关属性
		reportDir     设置测试报告的目录
		resultsDir     设置测试结果的目录
		aaptOptions：设置AAPT的属性
		failOnMissingConfigEntry     Forces aapt to return an error if it fails to find an entry for a configuration.
		ignoreAssets     Pattern describing assets to be ignore.
		noCompress     Extensions of files that will not be stored compressed in the APK.
		useNewCruncher     Whether to use the new cruncher.
		lintOptions：设置Lint的属性
		abortOnError     设置是否在lint发生错误时终止构建
		absolutePaths     Whether lint should display full paths in the error output. By default the paths are relative to the path lint was invoked from.
		check     The exact set of issues to check, or null to run the issues that are enabled by default plus any issues enabled via LintOptions.getEnable() and without issues disabled via LintOptions.getDisable(). If non-null, callers are allowed to modify this collection.
		checkAllWarnings     Returns whether lint should check all warnings, including those off by default.
		checkReleaseBuilds     Returns whether lint should check for fatal errors during release builds. Default is true. If issues with severity "fatal" are found, the release build is aborted.
		disable     The set of issue id's to suppress. Callers are allowed to modify this collection.
		enable     The set of issue id's to enable. Callers are allowed to modify this collection. To enable a given issue, add the issue ID to the returned set.
		explainIssues     Returns whether lint should include explanations for issue errors. (Note that HTML and XML reports intentionally do this unconditionally, ignoring this setting.)
		htmlOutput     The optional path to where an HTML report should be written.
		htmlReport     Whether we should write an HTML report. Default true. The location can be controlled by LintOptions.getHtmlOutput().
		ignoreWarnings     Returns whether lint will only check for errors (ignoring warnings).
		lintConfig     The default configuration file to use as a fallback.
		noLines     Whether lint should include the source lines in the output where errors occurred (true by default).
		quiet     Returns whether lint should be quiet (for example, not write informational messages such as paths to report files written).
		severityOverrides     An optional map of severity overrides. The map maps from issue id's to the corresponding severity to use, which must be "fatal", "error", "warning", or "ignore".
		showAll     Returns whether lint should include all output (e.g. include all alternate locations, not truncating long messages, etc.)
		textOutput     The optional path to where a text report should be written. The special value "stdout" can be used to point to standard output.
		textReport     Whether we should write an text report. Default false. The location can be controlled by LintOptions.getTextOutput().
		warningsAsErrors     Returns whether lint should treat all warnings as errors.
		xmlOutput     The optional path to where an XML report should be written.
		xmlReport     Whether we should write an XML report. Default true. The location can be controlled by LintOptions.getXmlOutput().
		check(id)     Adds the id to the set of issues to check.
		check(ids)     Adds the ids to the set of issues to check.
		disable(id)     Adds the id to the set of issues to enable.
		disable(ids)     Adds the ids to the set of issues to enable.
		enable(id)     Adds the id to the set of issues to enable.
		enable(ids)     Adds the ids to the set of issues to enable.
		error(id)     Adds a severity override for the given issues.
		error(ids)     Adds a severity override for the given issues.
		fatal(id)     Adds a severity override for the given issues.
		fatal(ids)     Adds a severity override for the given issues.
		ignore(id)     Adds a severity override for the given issues.
		ignore(ids)     Adds a severity override for the given issues.
		warning(id)     Adds a severity override for the given issues.
		warning(ids)     Adds a severity override for the given issues.
		dexOptions
		incremental     Whether to enable the incremental mode for dx. This has many limitations and may not work. Use carefully.
		javaMaxHeapSize     Sets the -JXmx* value when calling dx. Format should follow the 1024M pattern.
		jumboMode     Enable jumbo mode in dx (--force-jumbo).
		preDexLibraries     Whether to pre-dex libraries. This can improve incremental builds, but clean builds may be slower.
		compileOptions：设置编译的相关属性
		sourceCompatibility     Language level of the source code.
		targetCompatibility     Version of the generated Java bytecode.
		packagingOptions：设置APK包的相关属性
		excludes     The list of excluded paths.
		pickFirsts     The list of paths where the first occurrence is packaged in the APK.
		exclude(path)     Adds an excluded paths.
		pickFirst(path)     Adds a firstPick path. First pick paths do get packaged in the APK, but only the first occurrence gets packaged.
		jacoco：设置JaCoCo的相关属性
		version     设置JaCoCo的版本
		splits：设置如何拆分APK（比如你想拆分成arm版和x86版）
		abi     ABI settings.
		abiFilters     The list of ABI filters used for multi-apk.
		density     Density settings.
		densityFilters     The list of Density filters used for multi-apk.

dependencies：配置依赖

参考文档：
		最权威的官方打包指南（需要翻墙）http://tools.android.com/tech-docs/new-build-system
		Android打包插件API（在线版）http://apdr.qiniudn.com/index.html 

如果你对于手动配置build.gradle文件还不太熟练，那么可以使用AS提供的图形界面来配置，按下CMD+;即可打开配置页面


新特性：
     Google在用Gradle最为Android打包工具的时候引入了applicationId的概念，这是为了打多个不同ID的APK包准备的。
applicationId可以和清单文件中的packageName不一样，我们在代码中通过getPackageName()方法拿到的是applicationId，而清单文件中配置的packageName则仅作为R.java和BuildConfig.java的存放目录。
这样一来通过Class.forName(getPackageName()+”.R”)来获取R类的方式就行不通了，一定要注意。

打包

build.gradle文件配置完成后，打开终端，进入项目目录下，执行gradle build即可打包，打包结束后在相应module的build/outputs/apk/目录下可以看到.apk文件

如果你是在项目目录下执行的打包命令，那么会对项目中所有的module都打包，进入某个module目录下执行打包命令就只对当前module打包，每个module打包生成的APK都才存放在mudule的build/outputs/apk目录下





