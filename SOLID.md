   ### `SOLID` Principles

   `S` -> `Single responsibility`  ========> Separation of concerns
           every class or object should do only one thing

   `O` -> `Open closed principle`
        entities should be
        open for extension ' (Extend) ' and,
        closed for modification.

   `L` -> `Liskov substitution principle`
        entities of type T should be replaceable with their siblings
       Objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program


   `I` -> `Interface segregation`
        if an interface had too many features that doesn't relate to each others or must be implemented with each others,
        it must get segregated or divided


   `D` -> `Dependency inversion`
        entities should not depend on concrete implementation rather than depend on abstraction
        One should "depend upon abstractions, [not] concretions.





            - why composition over inheritance ?
               - to allow us to depend on injections -> for reusability

               - cuz composition allow us to follow the Dependency inversion principle
               - and that allows us to fake our implementation for TESTING.





             - we should follow these steps and rules from the beginning.
             - those rules depends on the correctness of our system design.
             - it's okay to refactor our code in the future.


 ##### OPEN CLOSED

``` kotlin
class LoginPage(var authManager: AuthManager) {

    fun facebookLogin() {
        var loginPage2: LoginPage = LoginPage(FacebookAuthManager())
        loginPage2.facebookLogin()
    }

    fun simpleLogin() {
        authManager.login()
    }
}

open class AuthManager() {

    open fun login() {
        // call ll api
        // call facebook
    }

    fun register() {}
    fun forgetPassword() {}

    // CLOSED for modification
    // add to new function -> Login by facebook
}

class FacebookAuthManager() : AuthManager() {

    // Open for extension

    override fun login() {

        super.login()
    }

    fun getLoginToken() {

    }

    fun getUserPermissions() {

    }

}

val facebookAuthManager = FacebookAuthManager()

fun FacebookAuthManager.retryLogin() {

}

fun sss() {
    facebookAuthManager.register()
    facebookAuthManager.getLoginToken()
    facebookAuthManager.retryLogin()
}
```



##### Liskov 

``` kotlin



interface AuthInterface {
    fun login()
    fun register()
    fun forget()
}

class FacebookAuthImplementation : AuthInterface {
    override fun login() {
        TODO("Not yet implemented")
    }

    override fun register() {
        TODO("Not yet implemented")
    }

    override fun forget() {
        // nothing happens here
    }

}

class DefaultAuthImplementation : AuthInterface {
    override fun login() {

    }

    override fun register() {
    }

    override fun forget() {
        TODO("Not yet implemented")
    }

}
```

##### Interface segregation

``` kotlin 


// WRONG
/*interface responseCallbacks {

    fun onCountriesResponse(value: String = "")
    fun onCitiesResponse(value: String = "")

    fun onLocationsResponse(value: String = "")
    fun onStreetResponse(value: String = "")
    fun onDefaultSettingsResponse(value: String = "")
    fun setSettingsResponse(value: String = "")
}*/

interface responseCallbacks {
    fun onCountriesResponse(value: String = "")
    fun onCitiesResponse(value: String = "")
}

interface anotherCallBack {
    fun onLocationsResponse(value: String = "")
    fun onStreetResponse(value: String = "")
    fun onDefaultSettingsResponse(value: String = "")
    fun setSettingsResponse(value: String = "")
}

class RegisterRepository() {

    var responseCallbacks: responseCallbacks? = null
    var anotherCallBack: anotherCallBack? = null

    fun setListener(callback: responseCallbacks) {
        responseCallbacks = callback
    }

    fun setListenerforAnotherCallback(callback: responseCallbacks) {
        responseCallbacks = callback
    }

    fun getCountries() {
        responseCallbacks?.onCountriesResponse("response")
    }

    fun getCities() {
        responseCallbacks?.onCitiesResponse("")
    }
}

class RegisterActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val registerRepository = RegisterRepository()
        registerRepository.setListener(object : responseCallbacks {
            override fun onCountriesResponse(value: String) {

            }

            override fun onCitiesResponse(value: String) {

            }

        })
    }
}
```
##### dependency inversion

``` kotlin
interface AuthManagerInterface {
    fun login()
    fun register()
    fun forget()
}

// VIEW
class AuthPage() {
    fun makeAuth() {

        // repository

        val authManagerRepository = authManagerRepository(FacebookAuthInterfaceImplementation())

        // for testing
        val authManagerRepository2 = authManagerRepository(FakeFacebookImpl())
        authManagerRepository.login()

    }
}


//viewmodel (reposioty) // composition /// testable
//viewmodel : repository // inheritance // not testable

// REPOSITORY
class authManagerRepository(val impl: AuthManagerInterface) {

    fun login() {
        impl.login()
    }
}


class AuthManagerInterfaceImplementation : AuthManagerInterface {
    override fun login() {
        TODO("Not yet implemented")
    }

    override fun register() {
        TODO("Not yet implemented")
    }

    override fun forget() {
        TODO("Not yet implemented")
    }

}

class FakeFacebookImpl : AuthManagerInterface {
    override fun login() {
        "login successfull"
    }

    override fun register() {
        "done"
    }

    override fun forget() {
        "sent"
    }

}

class FacebookAuthInterfaceImplementation : AuthManagerInterface {
    override fun login() {
        TODO("Not yet implemented")
    }

    override fun register() {
        TODO("Not yet implemented")
    }

    override fun forget() {
        TODO("Not yet implemented")
    }

}

```
