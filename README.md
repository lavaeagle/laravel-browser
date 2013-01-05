laravel-browser
===============

Easy to access and read browser info

Read the browser class itself for all the info you can get.

/application/bundles.php

```
<?php
return array(
	'browser' => array('auto'=>true)
);
```

/application/bundles/browser/start.php

```
<?php

Autoloader::map(array(
	'Browser' => Bundle::path('browser').'browser.php'
));

Laravel\IoC::singleton('Browser', function()
{
	return new Browser($_SERVER['HTTP_USER_AGENT']);
});
```

Example usage: /application/routes.php

    Route::filter('before', function()
    {

        /*
        *   Track a user's visit
        */

        // Create the browser object
		Bundle::start('Browser');
		$browser = IoC::resolve('Browser');

        // Create the Eloquent object Visit
        $visit = new Visit;

        // Obviously for illegal purposes only
        $visit->location = Locate::get( 'city' ) . ', ' . Locate::get( 'state' ) . ', ' . Locate::get( 'country' );
        $visit->ip_address = Request::ip();

        $visit->request = URI::current();
        if( Auth::check() )
            $visit->user_id = Auth::user()->id;

        // Browser stats
        $visit->browser = $browser->getBrowser();
        $visit->browser_version = $browser->getVersion();
        $visit->platform = $browser->getPlatform();
        $visit->mobile = $browser->isMobile();
        $visit->robot = $browser->isRobot();
        $visit->save();

    });
    
    
The Browser Class is created by:
http://chrisschuld.com/projects/browser-php-detecting-a-users-browser-from-php/