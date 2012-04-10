CheckPlayer FAQ

Q: What can CheckPlayer do that SWFObject can't do (by itself)?
A: The primary design goals of CheckPlayer were to augment SWFObject's functionality in the following ways:

    to make the Express Install feature accessible as a separate control flow and controllable DOM object - CheckPlayer allows you to control the auto-update, receive status "event" callbacks as it proceeds, and CSS style or DOM reposition the Update SWF's container. SWFObject requires that an Update SWF happen in place of a SWF asset and only after that SWF is called to be added to the page. As the dynamic SWFObject embedSWF() (which exposes the Express Install feature set) has some limitations which make it undesirable to work with in certain circumstances (dynamic script injection), CheckPlayer re-implemented this functionality, and in doing so, I believe opened up much more flexibility to this feature.
    to create a SWF embed feature that was more flexible, including being able to be called (and queued) at any time during a page's lifetime, and to have progress "event" callbacks, at least one of which indicated complete, successful loading of the SWF - CheckPlayer has the DoSWF() function which emulates some of the behavior of embedSWF(). DoSWF()'s parameters are identical to embedSWF() except that it adds one more optional parameter on the end, an "options" parameter which allows specifying any of several options, including a callback function to receive progress event callbacks as the SWF loads, a timeout value to cancel a load attempt after a certain interval, and a function-name (string value) to check a loaded SWF for ExternalInterface callback readiness.

    SWFObject only has a single boolean return value for the createSWF() function only, which merely indicates that the SWF object was added to the page's DOM. Additionally, embedSWF() cannot be called if the script is dynamically injected into a page. Lastly, createSWF() has no Express Install feature built into it, and it must be called after the DOM of the page is ready. DoSWF() combines the best of both these functions into a flexible, powerful, dynamic SWF embedding mechanism.

Q: What can SWFObject do that CheckPlayer can't do?
A: SWFObject has both static publishing (for standards compliance with markup, progressive enhancement, etc) and dynamic publishing options. CheckPlayer only allows for dynamic publishing (Javascript driven). It's important to remember that CheckPlayer *uses* the full SWFObject library, so actually an author can mix both static publishing from SWFObject alone with better dynamic publishing through CheckPlayer, on the same page if so desired!

Q: What if I am already using SWFObject on my page?
A: CheckPlayer relies on several features and bug fixes (including serious memory leak problems) only present in SWFObject 2.1+. If you are using any other version of SWFObject (2.0 or earlier), please upgrade to that before using CheckPlayer. If you are using SWFObject 2.1 on your page, you may directly link to SWFObject as a <script> tag reference in the HEAD of your page, as long as you make sure it comes before a reference to any flensed project files. The best approach however is to let CheckPlayer load SWFObject for you. Note: The full SWFObject library will still be available for your code to use even if you let CheckPlayer load it.

Q: Why is CheckPlayer a singleton object?
A: Because a browser can only have one version of a plugin installed at once, a web author must choose to target a single Flash Player version for each single page or site. As such, there is no valid reason to check for (and initiate updates of) multiple plugin versions. So, CheckPlayer conserves browser resources by forcing itself to only be able to be instantiated once (even if a web author attempts multiple instantiations). The check(-and-update) which is performed on the first instantiation should be considered valid once for the entire page's lifetime. Furthermore, if a web author really does need to do multiple version checks for some reason, the SWFObject API External Link provides appropriate tools for doing so.

Q: What happens if the browser plugin is updated?
A: Adobe's Express Install feature, available from Flash Player 6.0.65+, asks the user if they want to upgrade, and if they say 'Yes', it automatically downloads the latest Adobe Flash Player plugin for that browser and installs it. It then prompts the user to close the window, which the installer will then take care of re-opening to the original page URL. The resulting reload of your page will now have the appropriate latest plugin installed, and your re-check of the version should succeed, in which case your SWF assets should be displayed properly.

Q: How do I know what version to check for?
A: It depends on what version you targeted your SWF assets to. Odds are, version 8 or 9 was the target if the SWF is less than a year or two old. Check your "Publish" settings, or the documentation for the SWF asset if it was made by someone else, to be sure. If in doubt, check for version "9" (or "9.0.124") which is the most recent available from Adobe at the time of this writing. You can supply a shortened version string to CheckPlayer (ie, "9" instead of "9.0.124") if you only want to check for the major plugin version. If however your SWF asset is taking advantage of a particular build's features, such as the security model fully implemented in 9.0.124, then you can be more specific with what version you check for.

Q: Can I specifically ask for the player to update to a particular version?
A: No, Adobe's Express Install feature will automatically ask for the latest stable plugin version available for that user's platform/browser.

Q: What about Beta or Testing versions of the plugin?
A: At this time, it does not appear that Adobe makes available through Express Install anything other than their stable, production release builds. Furthermore, this is probably the best decision as it gives the most reliability to the broad web audience. If you need your assets to run in the Beta or Testing versions of the plugin, it's best for this to be done manually for any affected user environments. 