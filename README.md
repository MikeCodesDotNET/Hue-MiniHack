# Hue-MiniHack

Let's insert some marketing fluff about how amazing Philip Hue is. 

Before you start, you'll want to signup to the [Philips Hue Developer](http://www.developers.meethue.com) program which unlocks detailed API information and wealth or resources t for expanding your knowledge of Hue. 

> You'll find TODOs within the solution which numbers match this readme. You can find the Task pad in Xamarin Studio by clicking View > Pads > Tasks 

## The Challenge
You'll need to control Hue lamps from a Xamarin.Forms app. 

### Whitelisting and permission to access the bridge

1. #### Set your Apps name and Device name. 
    You should create a unique name (Mini-Hack is probably already taken) along with either a unique DeviceName or match your AppName. 

2. #### Locate the Hue Bridge
   Locate the BridgeViewModel class and add the following starting at line 60.
  ```csharp
   IBridgeLocator locator = new HttpBridgeLocator();
   IEnumerable<string> bridgeIPs = await locator.LocateBridgesAsync(TimeSpan.FromSeconds(5));

   BridgeIps.Clear();
   foreach (var ip in bridgeIPs)
   {
       BridgeIps.Add(ip);
   }
   ```
3. #### Register your app
  Locate the BridgeRegisterViewModel class and add the following starting at line 30.
  
  ```csharp
   ILocalHueClient client = new LocalHueClient(SelectedBridgeIP);
   
   if(string.IsNullOrEmpty(Helpers.Settings.AppKey))
       Helpers.Settings.AppKey = await client.RegisterAsync(App.AppName, App.DeviceName);
       
   client.Initialize(Helpers.Settings.AppKey);
   ```   
4. #### Create LocalHueClient
  Locate the LightsViewModel class and add the following starting at line 63.
  
  ```csharp
   ILocalHueClient client = new LocalHueClient(Helpers.Settings.DefaultBridgeIP);
   client.Initialize(Helpers.Settings.AppKey);
   ```      
 5. #### Discover all Hue lamps connected to bridge
  Staying in the LightsViewModel class, add the following after the client.Initialize method.
  
  ```csharp
   IEnumerable<Light> lights = await client.GetLightsAsync();
   Lights.Clear();
   foreach (var light in lights)
   {
      Lights.Add(light);
  }
   ```       
    
