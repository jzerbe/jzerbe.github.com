---
layout: post
title: Embedded Jetty6 on Android serving content from APK
tags:
- Jetty6
- Android
- APK
- J2EE
published: true
---
No matter what anybody tells you, it is possible to embed vanilla Jetty6 with
Servlet support in an Android application and serve all the content from
within the APK. It just requires a static content proxy Servlet.

First off, be sure to add `jetty-6.1.26.jar`, `jetty-util-6.1.26.jar`,
and `servlet-api-2.5-20081211.jar` into your Android `libs` directory
and classpath so they make their way into the APK\'s classpath when built.
You can get them in [jetty-6.1.26.zip](http://dist.codehaus.org/jetty/jetty-6.1.26/jetty-6.1.26.zip).

I am rocking a singleton instance to control my embedded Jetty6 server:

    public class JettyWebServer {
        
        private static JettyWebServer jws_SingleInstance = null;
        protected Server jws_serverInstance = null;
        protected static int jws_serverPortInt = 2320;
        
        protected JettyWebServer(int theServerPortInt) {
            jws_serverInstance = new Server(theServerPortInt);
            jws_serverPortInt = theServerPortInt;
            
            ServletHandler aServletHandler = new ServletHandler();
            aServletHandler.addServletWithMapping(new ServletHolder(new CoolServlet1()), "/servlets/cool1");
            aServletHandler.addServletWithMapping(new ServletHolder(new CoolServlet2()), "/servlets/cool2");
            
            ...
            
            aServletHandler.addServletWithMapping(new ServletHolder(new StaticProxy()), "/*");
            jws_serverInstance.addHandler(aServletHandler);
        }
        
        public static JettyWebServer getInstance() {
            if (jws_SingleInstance == null) {
                jws_SingleInstance = new JettyWebServer(jws_serverPortInt);
            }
            
            return jws_SingleInstance;
        }
        
        ...
        
    }

What is this `StaticProxy` you might ask? Well it takes any requests not handled by the _Cool Servlets_
and tries to find matching resources in the `assets` directory of the APK.

    public class StaticProxy extends HttpServlet {
        
        private static final long serialVersionUID = 1L;
        protected static HashMap<String, String> myExtensionMap;
        
        @Override
        public void init(ServletConfig config) throws ServletException {
            super.init(config);
            
            myExtensionMap = new HashMap<String, String>();
            myExtensionMap.put("css", "text/css");
            myExtensionMap.put("html", "text/html");
            myExtensionMap.put("ico", "image/vnd.microsoft.icon");
            
            ...
            
            myExtensionMap.put("png", "image/png");
        }
        
        @Override
        protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException {
            String aRequestedUri = req.getRequestURI();
            
            if ((aRequestedUri == null) || (aRequestedUri.equals(""))) {
                resp.sendError(400, "Missing resource string");
            } else {
                if (aRequestedUri.equals("/")) {
                    aRequestedUri = aRequestedUri + "index.html";
                }
                
                String[] aRequestedUriArray = aRequestedUri.split("/");
                String aRequestedFileName = aRequestedUriArray[(aRequestedUriArray.length - 1)];
                String[] aRequestedFileNameArray = aRequestedFileName.split("\.");
                String aRequestedFileExtension = aRequestedFileNameArray[(aRequestedFileNameArray.length - 1)];
                String aRequestAssetsPath = "webapp" + aRequestedUri;
                
                InputStream aResourceInputStream = MainActivity.getAssets().open(aRequestAssetsPath);
                resp.setContentType(myExtensionMap.get(aRequestedFileExtension));
                
                byte[] aBuffer = new byte[4096];
                int aBytesReadCount = 0;
                while ((aBytesReadCount = aResourceInputStream.read(aBuffer)) != -1) {
                    resp.getOutputStream().write(aBuffer, 0, aBytesReadCount);
                }
                
                aResourceInputStream.close();
                
                resp.getOutputStream().flush();
                resp.getOutputStream().close();
            }
        }
    }

The `MainActivity.getAssets()` static method returns an
[`AssetManager`](http://developer.android.com/reference/android/content/res/AssetManager.html) instance I previously
initialized from my main [`Activity`](http://developer.android.com/reference/android/app/Activity.html) in its
`onCreate` method with a call to `getAssets()`.
