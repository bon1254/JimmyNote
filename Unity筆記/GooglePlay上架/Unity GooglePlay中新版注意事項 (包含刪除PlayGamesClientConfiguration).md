1.  Hey all Just just going to pin this here quickly. If anyone experiences issues when either upgrading GPGS to the newest version 0.11.01 or [is](http://www.google.com/search?q=is+msdn.microsoft.com) starting the project after that point they've made some major changes, primarily that you no longer need to Initialize or Activate in order to call the authentication.
    

3.  Note: this is for the messages about PlayGamesClientConfiguration not existing, it is no longer required!
    

5.  Note for 0.11.1
    

7.  The new SDK contains four major changes to increase sign-in success which you
    
8.  should be aware of:
    

10.  1. Sign-in is triggered automatically when your game is launched. Creating a
    
11.  PlayGamesClientConfiguration instance, the initialization and activation of
    
12.  PlayGamesPlatform are not needed. Just calling
    
13.  `PlayGamesPlatform.Instance.Authenticate()` will fetch the result of automatic
    
14.  sign-in.
    
15.  2. Authentication tokens are now provided using
    
16.  `PlayGamesPlatform.Instance.requestServerSideAccess()`.
    
17.  3. Sign-out method is removed, and we will no longer require an in-game button
    
18.  to sign-in or sign-out of Play Games Services.
    
19.  4. Extra scopes cannot be requested.
    

21.  Before importing the unity package for v0.11.1, delete everything under
    
22.  `Assets/GooglePlayGames`. You can then follow the regular integration process.
