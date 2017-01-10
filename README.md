# ProtractorJS
## ProtractorJS to Test Node Applications

### 1. Configuration with bower

To configure protractor with bower add these packages in bower.json
     
     "protractor": "^4.0.11"
     
     "protractor-jasmine2-html-reporter": "0.0.6"

Now install these packages by running following command

     bower install

if above command gives an error than run below command

     sudo bower install
     
Now install webdriver manager or create a task in gulp task runner to install and run webdriver manager

     sudo webdriver-manager update
     
     sudo webdriver-manager start


OR

     gulp.task('webdriver-update', $.protractor.webdriver_update);

     gulp.task('webdriver-standalone', $.protractor.webdriver_standalone);

     // Protractor with selenium configuration to open browser

     //run webdriver method
     
     function runWebdriver(callback) {
       spawn('webdriver-manager', ['start'], {
         stdio: 'inherit'
       }).once('close', function(){ callback });
     }
     
     
Now create protractor configuration file for example confg.js or protractor.config.js or something like that

and add configurations contenet in that file like


    exports.config = {

      directConnect: true,

      baseUrl: 'url OF THE APP EITHER ITS ',

      capabilities: {
        browserName: 'chrome'
      },

      framework: 'jasmine',

      suites: {
        smoke: ['SMOKE TEST CASE FILE PATH'],
        regression: ['REGRESSION TEST CASE FILE PATH']
      },

      allScriptsTimeout: 30000, // TIMEOUT TO RUN A WAIT A TEST CASE NORMALLY

      jasmineNodeOpts: {
        showColors: true,
        defaultTimeoutInterval: 30000,
        isVerbose: true
      },
      
      // onPrepare Function here you can add the methods, variables which you want to use on prepareing the configurations
      
      onPrepare: function() {
        //repotert normally use to take screenshot or genertae HTML report etc
         
        /*const Jasmine2HtmlReporter = require('protractor-jasmine2-html-reporter'); 
        const specReporter = require('jasmine-spec-reporter');

        jasmine.getEnv().addReporter(new specReporter());
        jasmine.getEnv().addReporter(new Jasmine2HtmlReporter({
            savePath: './reports/',
            screenshotsFolder: 'images',
            takeScreenshotsOnlyOnFailures: true
        }));
        */
        browser.ignoreSynchronization = false;
        browser.driver.manage().window().setSize(1440, 900);
      }
    };


Create test case file which you want to create for different suites

Now you can and suit like

    I am calling smoke suite you can call any suite 
    
    protractor file_path/conf.js --suite=smoke

You can create a task to run test case suite like


    function runWebdriver(callback) {
      spawn('webdriver-manager', ['start'], {
        stdio: 'inherit'
      }).once('close', function(){ callback });
    }

    //run protractor configurations method

    function runProtractorSeleniumConfig(path) {
      gulp.src(getFile(path))
        .pipe(protractor({
            configFile: 'CONFIG_PATH/dev.conf.js'
        }))
        .on('error', function (e) {
            throw e;
        });
    }

    function getFile(ext){
      if(ext == '--regression'){
        return 'FILE_PATH/*.spec.js'
      }
      else{
        return 'FILE_PATH/*.spec.js'
      }
    }
    
    //execute protractor.config after webdriver is executed
  
    function runWebdriverProtractor(str){
      // runWebdriver(runWebdriver);
      runWebdriver(runProtractorSeleniumConfig(str));
    }

    //run smoke test commands configurations
    
    gulp.task('e2e:smoke-test', function(){
      runWebdriverProtractor(process.argv[3])
    });


Above configuration can handle both suites regression and smoke now you can run simply.


    gulp e2e:smoke-test  --regression       // for regression suite
    
    gulp e2e:smoke-test  --smoke             // for smoke suite
    
    
    
