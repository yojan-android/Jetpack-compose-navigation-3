# Jetpack Compose Navigation 3 - Type Safe Navigation

# Dependency Setup

## libs.versions.toml

```toml
[versions]
agp = "9.2.1"
coreKtx = "1.10.1"
junit = "4.13.2"
junitVersion = "1.1.5"
espressoCore = "3.5.1"
lifecycleRuntimeKtx = "2.6.1"
activityCompose = "1.8.0"
kotlin = "2.2.10"
composeBom = "2026.02.01"
navVersion = "2.9.8"

[libraries]
androidx-compose-material-icons-extended = { module = "androidx.compose.material:material-icons-extended" }

androidx-core-ktx = { 
    group = "androidx.core", 
    name = "core-ktx", 
    version.ref = "coreKtx" 
}

androidx-navigation-compose = { 
    module = "androidx.navigation:navigation-compose", 
    version.ref = "navVersion" 
}

junit = { 
    group = "junit", 
    name = "junit", 
    version.ref = "junit" 
}

androidx-junit = { 
    group = "androidx.test.ext", 
    name = "junit", 
    version.ref = "junitVersion" 
}

androidx-espresso-core = { 
    group = "androidx.test.espresso", 
    name = "espresso-core", 
    version.ref = "espressoCore" 
}

androidx-lifecycle-runtime-ktx = { 
    group = "androidx.lifecycle", 
    name = "lifecycle-runtime-ktx", 
    version.ref = "lifecycleRuntimeKtx" 
}

androidx-activity-compose = { 
    group = "androidx.activity", 
    name = "activity-compose", 
    version.ref = "activityCompose" 
}

androidx-compose-bom = { 
    group = "androidx.compose", 
    name = "compose-bom", 
    version.ref = "composeBom" 
}

androidx-compose-ui = { 
    group = "androidx.compose.ui", 
    name = "ui" 
}

androidx-compose-ui-graphics = { 
    group = "androidx.compose.ui", 
    name = "ui-graphics" 
}

androidx-compose-ui-tooling = { 
    group = "androidx.compose.ui", 
    name = "ui-tooling" 
}

androidx-compose-ui-tooling-preview = { 
    group = "androidx.compose.ui", 
    name = "ui-tooling-preview" 
}

androidx-compose-ui-test-manifest = { 
    group = "androidx.compose.ui", 
    name = "ui-test-manifest" 
}

androidx-compose-ui-test-junit4 = { 
    group = "androidx.compose.ui", 
    name = "ui-test-junit4" 
}

androidx-compose-material3 = { 
    group = "androidx.compose.material3", 
    name = "material3" 
}

kotlinx-serialization-json = {
    module = "org.jetbrains.kotlinx:kotlinx-serialization-json",
    version = "1.7.3"
}

[plugins]
android-application = { 
    id = "com.android.application", 
    version.ref = "agp" 
}

kotlin-compose = { 
    id = "org.jetbrains.kotlin.plugin.compose", 
    version.ref = "kotlin" 
}

```


```

plugins {
    alias(libs.plugins.android.application)
    alias(libs.plugins.kotlin.compose)

    kotlin("plugin.serialization") version "2.0.21"
}

android {
    namespace = "com.billing.jetpackcomposemdt"

    compileSdk {
        version = release(36) {
            minorApiLevel = 1
        }
    }

    defaultConfig {
        applicationId = "com.billing.jetpackcomposemdt"
        minSdk = 24
        targetSdk = 36

        versionCode = 1
        versionName = "1.0"

        testInstrumentationRunner =
            "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            isMinifyEnabled = false

            proguardFiles(
                getDefaultProguardFile(
                    "proguard-android-optimize.txt"
                ),
                "proguard-rules.pro"
            )
        }
    }

    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_11
        targetCompatibility = JavaVersion.VERSION_11
    }

    buildFeatures {
        compose = true
    }
}

dependencies {

    implementation(platform(libs.androidx.compose.bom))

    implementation(libs.androidx.activity.compose)
    implementation(libs.androidx.compose.material3)
    implementation(libs.androidx.compose.ui)
    implementation(libs.androidx.compose.ui.graphics)
    implementation(libs.androidx.compose.ui.tooling.preview)

    implementation(libs.androidx.core.ktx)
    implementation(libs.androidx.lifecycle.runtime.ktx)

    implementation(libs.androidx.navigation.compose)

    implementation(libs.kotlinx.serialization.json)

    implementation(
        libs.androidx.compose.material.icons.extended
    )

    testImplementation(libs.junit)

    androidTestImplementation(
        platform(libs.androidx.compose.bom)
    )

    androidTestImplementation(
        libs.androidx.compose.ui.test.junit4
    )

    androidTestImplementation(
        libs.androidx.espresso.core
    )

    androidTestImplementation(libs.androidx.junit)

    debugImplementation(
        libs.androidx.compose.ui.test.manifest
    )

    debugImplementation(
        libs.androidx.compose.ui.tooling
    )
}
```

```
package com.billing.jetpackcomposemdt.navigation

import kotlinx.serialization.Serializable

@Serializable
sealed class NavigationRoutes {

    @Serializable
    object HomeScreenUI : NavigationRoutes()

    @Serializable
    object DashboardScreenUI : NavigationRoutes()

    @Serializable
    data class WelcomeScreenUI(
        val userName: String? = null
    ) : NavigationRoutes()
}
```


```
package com.billing.jetpackcomposemdt.navigation

import androidx.compose.runtime.Composable
import androidx.navigation.compose.NavHost
import androidx.navigation.compose.composable
import androidx.navigation.compose.rememberNavController
import androidx.navigation.toRoute

@Composable
fun NavigationGraph() {

    val navController = rememberNavController()

    NavHost(
        navController = navController,
        startDestination = NavigationRoutes.HomeScreenUI
    ) {

        composable<NavigationRoutes.HomeScreenUI> {

            HomeScreen(navController)
        }

        composable<NavigationRoutes.DashboardScreenUI> {

            DashboardScreen(navController)
        }

        composable<NavigationRoutes.WelcomeScreenUI> { backStackEntry ->

            val data =
                backStackEntry.toRoute<
                    NavigationRoutes.WelcomeScreenUI
                >()

            WelcomeScreen(
                userName = data.userName
            )
        }
    }
}

```

```
package com.billing.jetpackcomposemdt.screens

import androidx.compose.material3.Button
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.navigation.NavHostController
import com.billing.jetpackcomposemdt.navigation.NavigationRoutes

@Composable
fun HomeScreen(
    navController: NavHostController
) {

    Button(
        onClick = {

            // Normal Navigation
            navController.navigate(
                NavigationRoutes.DashboardScreenUI
            )

            // Navigation With Data
            navController.navigate(
                NavigationRoutes.WelcomeScreenUI(
                    userName = "Yojan"
                )
            )

            // Remove Previous Screen From BackStack
            navController.navigate(
                NavigationRoutes.DashboardScreenUI
            ) {

                popUpTo(
                    NavigationRoutes.HomeScreenUI
                ) {
                    inclusive = true
                }
            }
        }
    ) {

        Text(text = "Navigate")
    }
}


```
```
package com.billing.jetpackcomposemdt.screens

import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.navigation.NavHostController

@Composable
fun DashboardScreen(
    navController: NavHostController
) {

    Text(text = "Dashboard Screen")
}
```

```
package com.billing.jetpackcomposemdt.screens

import androidx.compose.material3.Text
import androidx.compose.runtime.Composable

@Composable
fun WelcomeScreen(
    userName: String?
) {

    Text(
        text = "Welcome $userName"
    )
}


navController.navigate(
    NavigationRoutes.DashboardScreenUI
)


navController.navigate(
    NavigationRoutes.WelcomeScreenUI(
        userName = "Yojan"
    )
)
```
```
navController.popBackStack()
```
```
navController.navigate(
    NavigationRoutes.DashboardScreenUI
) {

    popUpTo(
        NavigationRoutes.HomeScreenUI
    ) {
        inclusive = true
    }
}

```

```
navController.navigate(
    NavigationRoutes.HomeScreenUI
) {

    popUpTo(0)
}

```


```
@Serializable
sealed class NavigationRoutes


@Serializable
object DashboardScreenUI : NavigationRoutes()


@Serializable
data class WelcomeScreenUI(
    val userName: String
) : NavigationRoutes()
```


Never Pass Large Objects

Avoid Passing:

Bitmap
ResponseModel
Large List
Full User Object

Pass Only:

id
name
small primitive values

Fetch actual data from:

ViewModel
Repository
API
Room Database



## AVoid doing 
LaunchedEffect(Unit) {
    apiCall()
}
