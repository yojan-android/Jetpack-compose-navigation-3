# Jetpack-compose-navigation-3
Dependency related jetpack compose and type safe navigaiton

## lib.version.toml

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
androidx-core-ktx = { group = "androidx.core", name = "core-ktx", version.ref = "coreKtx" }
androidx-navigation-compose = { module = "androidx.navigation:navigation-compose", version.ref = "navVersion" }
junit = { group = "junit", name = "junit", version.ref = "junit" }
androidx-junit = { group = "androidx.test.ext", name = "junit", version.ref = "junitVersion" }
androidx-espresso-core = { group = "androidx.test.espresso", name = "espresso-core", version.ref = "espressoCore" }
androidx-lifecycle-runtime-ktx = { group = "androidx.lifecycle", name = "lifecycle-runtime-ktx", version.ref = "lifecycleRuntimeKtx" }
androidx-activity-compose = { group = "androidx.activity", name = "activity-compose", version.ref = "activityCompose" }
androidx-compose-bom = { group = "androidx.compose", name = "compose-bom", version.ref = "composeBom" }
androidx-compose-ui = { group = "androidx.compose.ui", name = "ui" }
androidx-compose-ui-graphics = { group = "androidx.compose.ui", name = "ui-graphics" }
androidx-compose-ui-tooling = { group = "androidx.compose.ui", name = "ui-tooling" }
androidx-compose-ui-tooling-preview = { group = "androidx.compose.ui", name = "ui-tooling-preview" }
androidx-compose-ui-test-manifest = { group = "androidx.compose.ui", name = "ui-test-manifest" }
androidx-compose-ui-test-junit4 = { group = "androidx.compose.ui", name = "ui-test-junit4" }
androidx-compose-material3 = { group = "androidx.compose.material3", name = "material3" }

[plugins]
android-application = { id = "com.android.application", version.ref = "agp" }
kotlin-compose = { id = "org.jetbrains.kotlin.plugin.compose", version.ref = "kotlin" }









##build.gradle.kts (Application level)

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

        testInstrumentationRunner = "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            isMinifyEnabled = false
            proguardFiles(
                getDefaultProguardFile("proguard-android-optimize.txt"),
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
    testImplementation(libs.junit)
    androidTestImplementation(platform(libs.androidx.compose.bom))
    androidTestImplementation(libs.androidx.compose.ui.test.junit4)
    androidTestImplementation(libs.androidx.espresso.core)
    androidTestImplementation(libs.androidx.junit)
    debugImplementation(libs.androidx.compose.ui.test.manifest)
    debugImplementation(libs.androidx.compose.ui.tooling)
    implementation(libs.androidx.navigation.compose)
    implementation(libs.androidx.compose.material.icons.extended)
}



##Navigation Model 

@Serializable 
sealed class NavigationRoutes {
  @Serializable
  object HomeScreenUI : NavigationRoutes() 
  object DashboardUI : NavigationRoutes()



##Navigation Graph 

## import androidx.navigation.compose.NavHost -> NavHost import 
@Composable
fun NavigationGraph() {
  val navController = rememberNavController()

  NavHost(
  navcontroller = navcontroller,
  startDestination = NavigationRoutes.HomeScreenUI
  ) {
  composable<NavigationRoutes.HomeScreenUI> {
            HomeScreen(navController)
        }
        composable<NavigationRoutes.DashboardScreenUI> {
            DashboardScreen(navController)
      }
  }
}

## this are without data passing from one composable to another 
## incase when we want to pass data from one composable to another 


 @Serializable -> the sealed class will have data class when we want to carry data 
    data class WelcomeScreenUI(val userName : String? = null) : NavigationRoutes()

//backStackEntry is lambda function which pass data during navigation
        composable<NavigationRoutes.WelcomeScreenUI> { backStackEntry ->
            val data = backStackEntry.toRoute<NavigationRoutes.WelcomeScreenUI>()
            WelcomeScreen(data.userName)
        }


## And to collect that passed string or body will use fun parameter 

@composable
fun WelcomeScreen(userName: String?)
        
















