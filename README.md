  
# Official Proximity SDK for Native Android
&nbsp;
<p align="center" width="100%">
    <img width="33%" src="https://images.squarespace-cdn.com/content/v1/6073efa8ab5c5b78808fe878/1618336613080-9PJH9HELYSVDIBCPF7OU/logo.png?format=1500w">
</p>

&nbsp;

Setup
=====

*   ### Using offline .aar file
    

> Download the "aar" file and add it to your project.

    Project structure -> Dependency -> App Module -> Click + icon -> Provide your .aar file path -> click apply -> ok
    

> Android studio will automatically add the dependency from your local aar file path and will sync your project. After finishing the sync, you are ready to use the Proximity SDK in your project

*   ### RUN-TIME Permissions
    

> The following permissions are required in the application's Mainfest file for smooth working of the SDK services.

*   android.permission.INTERNET
*   android.permission.ACCESS\_FINE\_LOCATION
*   android.permission.ACCESS\_COARSE\_LOCATION
*   android.permission.FOREGROUND\_SERVICE
*   android.permission.ACCESS\_BACKGROUND\_LOCATION
*   android.permission.ACTIVITY\_RECOGNITION


Usage
=====

### 1. Initialise

> Your application should register with Proximity SDK before invoking the location updates method. You need to pass the accessToken provided for your tenant to authorize the app for further usage. forceInit should be passed as true if you want to forcefully kill all the background tasks and create a new one. Default value will be false.

     // Initialise Function
    
    fun init(org : String, forceInit: Boolean = false, callbacks : CallBacks, isTestMode : Boolean = false)
      
    

    
      //Implementation Example
    
      import co.deliverysolutions.proximity.ProximitySDK  // Import Proximity SDK package
    
      lateinit var proximitySDKObject: ProximitySDK // Create Proximty SDK object
    
    
    /*
    this code will return error if any previous order is running.
    you need to pass forceInit = true, if you want to forcefully registerfor new tracking.
    */
      proximitySDK.init("<YOUR-ACCESS-TOKEN>", object : ProximitySDK.CallBacks{
           override fun success(successMessage: String) {
    	        var isTracking : Boolean = proximitySDK.isLocationServiceRunning() // you can determine the SDK status with this method.
              // proceed further according to tracking Status value.
            }
           override fun onError(errorMessage: String) {
                // If there is any error encountered by the SDK this callback will be triggered.
            }
      },isTestMode = true)
    

Note : Please proceed with next step only if this register function success call back is received.

### 2. Register for location updates

### Register for location updates

    fun startLocationUpdates(orderID : String,callbacks: CallBacks, /*optional*/ locationInterval : Long = 15000)


    var orderID = "your order id"
    proximitySDK
      .startLocationUpdates(orderID, object : ProximitySDK.CallBacks{
         override fun success(success: String) {
             // On success the SDK will start updating location details.
             
         }

         override fun onError(error: String) {
             // Any errors on updating location details to the proximity Server will be recived here.
             // Any permission errors will also be recieved here.
         }
         
    },/*interval(optional)*/ 15000)

Note : This method will create a foreground service along with a
notification for accessing background location if the application is
killed.

### 3. Register for location updates Callbacks

    import co.deliverysolutions.proximity.ProximitySDK.SDKUpdates

    fun registerForSDKUpdates(sdkUpdates : SDKUpdates)


      proximitySDK.registerForSDKUpdates(object : SDKUpdates{
        override fun onStop(isStopped: Boolean) {
          Toast.makeText(applicationContext,isStopped,Toast.LENGTH_LONG).show()
        }

        override fun permissionError(error: String) {
          Toast.makeText(applicationContext,error,Toast.LENGTH_LONG).show()
        }

        override fun onError(error: String) {
          Toast.makeText(applicationContext,error,Toast.LENGTH_LONG).show()
        }

    })

### 4. Update Vehicle Info

User vehicle information can be shared with the SDK, which will be updated along with the location updates service.


User vehicle information can be shared with the SDK, which will be
updated along with the location updates service.

      //Update Vehicle Info function
      fun updateVehicleDetails(userInfo: UserInfo?,callbacks: CallBacks)
      

      var userInfo = UserInfo(
          vehicleColor = "Black",
          isPickupBySomeoneElse = false,
          contactNumber = "1234567890",
          parkingSlot = "A1",
          description = "Description",
          vehicleNumber = "KL-11-AY-4545",
          userName = "testuser",
          vehicleMake = "Toyota",
          vehicleType = "Sedan",
        )


        proximitySDK.updateVehicleDetails(orderID: String, userInfo,object : ProximitySDK.CallBacks{
            override fun success(success: String) {
              Toast.makeText(applicationContext,success,Toast.LENGTH_LONG).show()
              finish()
            }

            override fun onError(error: String) {
              Toast.makeText(applicationContext,error,Toast.LENGTH_LONG).show()
            }
        })
    )

### 5. Stop location updates

This method will forcefully stop location updates done by the SDK. Any pinned notification will also be removed from the notification bar.

    proximitySDK.stopLocationUpdates()
    

### 6. Check location updates status

This method will return a boolean value according to SDK tracking Status.

    var isBackgroundServiceRunning = proximitySDK.isLocationServiceRunning()
    
In case user rejects the location permission, Proximity SDK have these methods to update the status of the user such as Estimated time of arival, Arrived status and Received status.


### 7. Updating ETA


Estimated time of arival can be shared with SDK by using the below
method.

    fun updateETA(orderID: String, estimatedTime: String, callbacks: CallBacks)

      proximitySDK.addETA(orderID = {orderID},
          nowAsISO,object : CallBacks{
          override fun success(success: String) {
              Toast.makeText(applicationContext,success,Toast.LENGTH_LONG).show()
          }

          override fun onError(error: String) {
              Toast.makeText(applicationContext,error,Toast.LENGTH_LONG).show()
          }

      })

### 8. Arriving to the store

Arrived Status can be shared with SDK using the below method.

    fun setArrived(orderID: String, callbacks: CallBacks)

      proximitySDK.setArrived(orderID = {orderID}, object : CallBacks{
          override fun success(success: String) {
              Toast.makeText(applicationContext,success,Toast.LENGTH_LONG).show()
          }

          override fun onError(error: String) {
              Toast.makeText(applicationContext,error,Toast.LENGTH_LONG).show()
          }
      })


### 9. Completing an Order (Order Received)

      fun orderReceived(orderID: String, callbacks: CallBacks)

      proximitySDK.orderReceived(orderID = {orderID}, object : CallBacks{
          override fun success(success: String) {
              Toast.makeText(applicationContext,success,Toast.LENGTH_LONG).show()
          }

          override fun onError(error: String) {
              Toast.makeText(applicationContext,error,Toast.LENGTH_LONG).show()
          }
      })
      
## License

Copyright (c) 2022 by [Delivery Solutions](https://www.deliverysolutions.co). All Rights Reserved. Usage of this library implies agreement to abide by the license terms. Please refer to our [Terms of Service](https://www.deliverysolutions.co/terms-of-service).

