laravel-browser
===============

Easy to access and read browser info

Read the browser class itself for all the info you can get.
I did not write this class, if you know who did please tell me so I put them in here.

// Example useage: /application/routes.php
    Route::filter('before', function()
    {

        /*
        *   Track visit
        */
        $browser = new Browser;
        $visit = new Visit;
        $visit->location = Locate::get( 'city' ) . ', ' . Locate::get( 'state' ) . ', ' . Locate::get( 'country' );
        $visit->ip_address = Request::ip();
        $visit->request = URI::current();
        if( Auth::check() )
            $visit->user_id = Auth::user()->id;
        $visit->browser = $browser->getBrowser();
        $visit->browser_version = $browser->getVersion();
        $visit->platform = $browser->getPlatform();
        $visit->mobile = $browser->isMobile();
        $visit->robot = $browser->isRobot();
        $visit->save();

    });