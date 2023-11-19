public class DuckDuckGoPerformanceTest{
    public static void main(String[] args)
    {
    
    //Initialize Jmeter
    JMeterUtils.loadJMeterProperties("C:\Program Files\apache-jmeter-5.6.2\apache-jmeter-5.6.2\bin");
    JMeterUtils.initLocale();

    // Create test plan
    HashTree testplanTree=new HashTree();

    //Create Thread Group
    SetupThreadGroup threadGroup=new SetupThreadGroup();
    threadGroup.setNumThreads(1);
    threadGroup.setRampUp(60);
    threadGroup.setloop(300);
    
    

    //Create Loop Controller
    LoopController loopController=new LoopController();
    loopController.setLoops(300);

    //Create CSV Data Set Config
    CSVDataSet csvDataSet=new CSVDataSet();
    csvDataSet.setProperty("filename", "SearchTerms_DuckDuckGo.txt");
    csvDataSet.setProperty("variableName", "PlanIT");

    //HTTP Cookie Manager
    CookieManager cookieManager=new CookieManager();
    cookieManager.setnmae("HTTP Cookie Manager");
    cookieManager.setproperty("clearEachIteration", true);

    //HTTP Cache Manager
    CacheManager cacheManager=new CacheManager();
    CacheManager.setName("HTTP CacheManager");

        //Create Transaction Controller
    TransactionController transactionController=new TransactionController();
    transactionController.setName("Transaction Controller");
    transactionController.setproperty(TestElement.GUI_CLASS. TransactionControllerGui.class.getName());
    transactionController.setIncludeTimers(true);


    //Create HTTP Sampler for DuckDuckGo Homepage
    HTTPSamplerProxy openHomePageSampler=new HTTPSamplerpROXY();
    OpenHOmePageSampler.setDomain("duckduckgo.com");
    openHomePageSampler.setPath("/");
    OpenHOmePageSampler.setMethod("GET");

    //Create HTTP Sampler for Typing Suggested Term in DuckDuckGo 
    HTTPSamplerProxy typeSearchTermSampler=new HTTPSamplerProxy();
    typeSearchTermSampler.setDomain("duckduckgo.com");
    typeSearchTermSampler.setPath("/");
    typeSearchTermSampler.setMethod("POST");
    typeSearchTermSampler.addArgument("q", "${planIT}");


    //Create HTTP Sampler for Selecting search Term from suggested search box
    HTTPSamplerProxy selectSearchTermSampler=new HTTPSamplerProxy();
    selectSearchTermSampler.setDomain("duckduckgo.com");
    selectSearchTermSampler.setPath("/?va=t&t=hb&q=planit+australia&ia=web/");
    selectSearchTermSampler.setMethod("GET");



    //Add Samplers to the Transaction Controller
    transactionController.addTestElemet(OpenHOmePageSampler);
    transactionController.addTestElement(typeSearchTermSampler, planIT);
    transactionController.addTestElement(selectSearchTermSampler. planIT Australia);

    //Add elements to the Test Plan
    testplanTree.add(CSV Set Data Config);
    testplanTree.add(cookieManager);
    testplanTree.add(cacheManager);
    testplanTree.add(threadGroup, loopController);
    testplanTree.add(transactionController, loopController);

    //Add Result Collector
    ResultCollector resultcollector=new ResultCollector();
    resultCollector.setFilename("view results tree.jtl");
    SampleSDaveConfiguration saveconfig=new SampleSDaveConfiguration();
    saveconfig.setAsXml(true);
    resultcollector.setSaveConfig(saveConfig);
    testPlanTree.add(resultCollector);

    //Run the Test Plan
    SaveService.saveTree(testPlanTree, System.out);
    JmeterUtils.runTest(testPlanTree, false);

    //Save the Test Plan to a file
    SaveService.saveTree(testPlanTree, new FileOutputStream("DuckDuckGoPerformanceTest.jmx"));

}

}