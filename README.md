# Epicindustrie//github.com/orgs/Epicindustries/people?query=role%3Aowner

//help.github.com/articles/what-are-the-different-access-permissions/#organization-accounts


//Intitialise frame counter.
var frameCounter = 0;

//Called when application is started.
function OnStart()
{
    //Set full screen game mode.
    app.SetScreenMode( "Game" );
    
    //Create the main layout
    lay = app.CreateLayout( "Linear", "FillXY" );

    //Create a GLView and add it to layout.
    glview = app.CreateGLView( 1, 1, "Fast2d" );
    lay.AddChild( glview );

    //Add the main layout to the app.
    app.AddLayout( lay );
    
    //Set the droid width to be the width of the 
    //GLView and the correct aspect ratio.
    droidWidth = 1.0;
    droidHeight = droidWidth * glview.aspect;
    
    //Create the droid image to draw in the GLView and
    //call StartRendering once Hello.png has finished loading
    imgDroid = glview.CreateImage( "/Sys/Img/Hello.png", StartRendering );
}

function StartRendering()
{
    //Render at 30 frames per second
    setInterval( DrawFrame, 1000/30 );
}

function DrawFrame()
{
    //Position the un-scaled droid so that it is
    //centered in the GLglview.
    var x = 0.5 - (droidWidth/2);
    var y = 0.5 - (droidHeight/2);    
    
    //Increase the droid angle by 10 degrees each frame.
    var angle = (frameCounter*10);

    //Scale the droid each frame using Math.sin to give
    //a bouncing effect
    var scale = 0.1 + Math.abs(Math.sin(frameCounter/10));
    var scaledDroidWidth = droidWidth * scale;
    var scaledDroidHeight = droidHeight * scale;
            
    //Offset x and y so that the droid remains at the
    //center of the GLView now it has been scaled.
    x = x - ((scaledDroidWidth/2) - (droidWidth/2));
    y = y - ((scaledDroidHeight/2) - (droidHeight/2));
    
    //Draw the droid.
    glview.DrawImage( imgDroid, x, y, 
        scaledDroidWidth, scaledDroidHeight, angle );
    
    //Render the graphics to screen.
    glview.Render();
    
    //Add 1 to frame counter.
    frameCounter++;
}


//Called when application is started.
function OnStart()
{
	//Create a layout with objects vertically centered.
	lay = app.CreateLayout( "linear", "VCenter,FillXY" );	

	//Create a web control.
	web = app.CreateWebView( 0.8, 0.8 );
	web.SetOnProgress( web_OnProgess );
	lay.AddChild( web );
	
	//Create horizontal sub-layout for buttons.
	layHoriz = app.CreateLayout( "linear", "Horizontal" );	
	
	//Create 'Local' button.
	btnLocal = app.CreateButton( "Local" );
	btnLocal.SetOnTouch( btnLocal_OnTouch );
	layHoriz.AddChild( btnLocal );
	
	//Create 'Dynamic' button.
	btnDynamic = app.CreateButton( "Dynamic" );
	btnDynamic.SetOnTouch( btnDynamic_OnTouch );
	layHoriz.AddChild( btnDynamic );
	
	//Create 'Remote' button.
	btnRemote = app.CreateButton( "Remote" );
	btnRemote.SetOnTouch( btnRemote_OnTouch );
	layHoriz.AddChild( btnRemote );
	
	//Add horizontal layout to main layout.
	lay.AddChild( layHoriz );
	
	//Add layout to app.	
	app.AddLayout( lay );
}

//Called when user touches 'Local' button.
//(We could use a url like "file:///sdcard/MyPage.htm")
function btnLocal_OnTouch()
{
	web.LoadUrl( "file:///Sys/Html/Page.htm" );
}

//Called when user touches 'Dynamic' button.
function btnDynamic_OnTouch()
{
	var html = "<html><head>";
	html += "<meta name='viewport' content='width=device-width'>";
	html += "</head><body>Hello Dynamic World!<br>";
	html += "<img src='Img/Droid2.png'>";
	html += "</body></html>";
	web.LoadHtml( html, "file:///Sys/" );
}

//Called when user touches 'Remote' button.
function btnRemote_OnTouch()
{
	app.ShowProgress("Loading...");
	web.LoadUrl( "http:///www.google.com" );
}

//Show page load progress.
function web_OnProgess( progress )
{
	app.Debug( "progress = " + progress );
	if( progress==100 ) app.HideProgress();
}
