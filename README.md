# Cordova Plugin Audit

- [Cordova Plugin Audit](#cordova-plugin-audit)
  - [What is the Cordova Plugin Audit](#what-is-the-cordova-plugin-audit)
  - [How Do End-Users Use This Service in Cordova CLI](#how-do-end-users-use-this-service-in-cordova-cli)
    - [Example Use Case - Plugin Add](#example-use-case---plugin-add)
      - [Plugin Add Fail Example Output](#plugin-add-fail-example-output)
      - [Plugin Add Pass Example Output](#plugin-add-pass-example-output)
    - [Example Use Case - Platform Add](#example-use-case---platform-add)
      - [Platform Add Fail Example Output](#platform-add-fail-example-output)
      - [Platform Add Pass Example Output](#platform-add-pass-example-output)
    - [Additional Use Case Notes](#additional-use-case-notes)
  - [Can End-Users Access Results from the Service website](#can-end-users-access-results-from-the-service-website)
  - [How Do End-Users Benefit the Cordova Plugin Audit](#how-do-end-users-benefit-the-cordova-plugin-audit)
  - [What does the Cordova Plugin Audit Test](#what-does-the-cordova-plugin-audit-test)
    - [Additional Test Notes](#additional-test-notes)
  - [How Much Data is Available](#how-much-data-is-available)
  - [How is a Test Triggered](#how-is-a-test-triggered)
  - [How Does The Cordova Plugin Audit Integrate with Cordova](#how-does-the-cordova-plugin-audit-integrate-with-cordova)
  - [Overall Goal](#overall-goal)
  - [What are Some of the Future Tasks & Steps](#what-are-some-of-the-future-tasks--steps)
    - [Possible Future Task within Cordova](#possible-future-task-within-cordova)
  - [What Might be End-Users Concerns](#what-might-be-end-users-concerns)

## What is the Cordova Plugin Audit

Cordova Plugin Audit is a tool and service for testing, gathering, and providing useful information of any given plugin to the end-user.

In essence, the Cordova Plugin Audit is similar to what npm audit does but the main difference is that it does not test for security. The plugin audit system aims to identify issues with the plugin during various stages of its lifecycle. Some stages are not planned to be introduced within the first phase of release.

Below outlines the stages that are tested by the audit system (currently).

[&#x1F51D;](#cordova-plugin-audit)

## How Do End-Users Use This Service in Cordova CLI

Users would just continue to use Cordova the same way they use it today.

A notification would appear after the plugin or platform add procedure. A link to the plugin test review or report when a failure was detected.

> Users may need to pass in a flag or configure to enable the service if this feature is to be released before next major. The goal is on next major, this feature is enabled by default.

### Example Use Case - Plugin Add

The user's project already has a platform installed and no plugins.

- `cordova-android@6.0.0`

The user types in the following command:

- `cordova plugin add cordova-plugin-camera`

CLI/Lib/Fetch will fetch and install `cordova-plugin-camera`. After the `plugin add` step has completed (passing or failing), an HTTPS request is sent to the service with the following information:

- `cordova-android@6.0.0`
- `cordova-plugin-camera@x.y.z` (the version is known after install)

The audit results, if available, is returned to the end-user and displayed on the screen.

#### Plugin Add Fail Example Output

```prompt
...
Usual Install Output
...

audit has detected an issue with plugin: cordova-plugin-camera@x.y.z
  See Plugin Test Result @ https://....io/hash-code-to-test
```

#### Plugin Add Pass Example Output

```prompt
...
Usual Install Output
...

audit has detected no issues.
```

### Example Use Case - Platform Add

The user's project already has a few plugins installed but no platform.

- `cordova-plugin-camera@1.0.0`
- `cordova-plugin-device@1.0.0`

The user types in the following command:

- `cordova platform add android`

CLI/Lib/Fetch will fetch and install `cordova-android`. After the `p add` step has completed (passing or failing), an HTTPS request is sent to the service with the following information:

- `cordova-android@x.y.z`  (the version is known after install)
- `cordova-plugin-camera@1.0.0`
- `cordova-plugin-device@1.0.0`

A collection of audit results, if available, is returned to the end-user and displayed on the screen.

#### Platform Add Fail Example Output

```prompt
...
Usual Install Output
...

audit has detected one or more plugin issue.
  See Report @ https://....io/hash-code-to-report
```

#### Platform Add Pass Example Output

```prompt
...
Usual Install Output
...

audit has detected no issues.
```

### Additional Use Case Notes

- Plugins, in the `package.json`, that contain links or file paths will not be sent to the server. These plugins would be considered private or development.

- Cordova does not send any identifiable information to the plugin audit service such as bundle identifiers, app id, app name, etc.

[&#x1F51D;](#cordova-plugin-audit)

## Can End-Users Access Results from the Service website

Outside of Cordova CLI, users can search for test result with the given test case parameters through the Audit service database (website).

![Audit Service Test Results Website Prototype](../master/example-test-results.png)

[&#x1F51D;](#cordova-plugin-audit)

## How Do End-Users Benefit the Cordova Plugin Audit

The major benefit that an end-user recieves from this system is a warning about the newly installed plugin when one or more defects are detected.

There are possibilities that the defects are within the functionality and is not detected within the audit system. The goal though is to try and identify where we can and report to the end-users to reduce time wasted in an attempt to use a bugged plugin.

[&#x1F51D;](#cordova-plugin-audit)

## What does the Cordova Plugin Audit Test

A single test is broken down into multiple tasks that run a single action (command, script, etc). This is for the ability to record and report at a granular level. (For example time of execution)

- **Initialize:** Enables the recording of overall execution time.

- **Display Environment Test Settings:** Displays test case parameters in the runtime log.

- **Set Environment Node Version:** Sets the node version that will be used for testing. This is an available test parameter that can be changed. (Defaults: latest LTS).

- **Create a Staging Directory:** The staging directory is the Cordova project directory.

- **Add Cordova CLI to Stage Directory:** Adds the given version of Cordova CLI to the staging directory. This is an available test parameter that can be changed. (Defaults: latest Cordova version) All future Cordova commands uses `npx`.

- **Append Platform:** Adds the given platform@version to the staging directory.

- **Add Plugin Test:** Adds the given plugin@version to the staging directory. This will test if plugin adds successfully including the plugin-add related hook scripts.

- **Display Cordova Requirements:** Displays Cordova requirements in the runtime log for reporting additional information.

- **Display Cordova Info:** Displays Cordova info in the runtime log for reporting additional information.

- **Build Test:** Runs a standard debug build with the no additional params or configurations. This tests if the build is successful which also includes build-related hook scripts.

- **Remove Plugin Test:** Remove the plugin from the staging directory. This will test if plugin removes successfully including the plugin-remove related hook scripts.

- **Cleanup Staging Directory:** Removes the staging directory.

- **Destroy:** Disables the recording of overall execution time

[&#x1F51D;](#cordova-plugin-audit)

### Additional Test Notes

- Only one plugin can be tested to reduce the potential risk of cross-plugin contamination.

- Only one platform can be tested to reduce the potential risk of cross-platform contamination.

[&#x1F51D;](#cordova-plugin-audit)

## How Much Data is Available

As the service begins, the quantity of the data is limited. This data depends on what test cases have run.

During the initial release phase, the service will test the latest version of each default Cordova plugin (as well as a few hand-picked popular plugins will be tested) against the current version of each supported platform.

After the first phase for testing is completed, additional plugins can be added to the hourly watch. As there are a lot of plugins the priority of which plugins are to be added is non-existence. Users would have the ability to request, via form, to try and get a plugin added. When a plugin is added, it will focus again on testing against the latest versions.

Older versions are not tested automatically.

[&#x1F51D;](#cordova-plugin-audit)

## How is a Test Triggered

The plugin audit system periodically checks the npm registry for a new version of each plugin that is on the plugin monitor list. The plugins that are released on the npm registry is deemed production-ready and this is why the system only monitors npm.

Additionally, it only monitors for the latest tag. It does not look at custom tags such as `rc`, `beta`, `dev`, etc..

[&#x1F51D;](#cordova-plugin-audit)

## How Does The Cordova Plugin Audit Integrate with Cordova

The integration would be within Cordova lib. During the `plugin add` and `platform add` step, if the plugin audit system is enabled, an HTTPS request would be made to the service for the results of what is being installed.

[&#x1F51D;](#cordova-plugin-audit)

## Overall Goal

In summary, the end goal is to provide end-users a better user experience so they can focus on developing an app with plugins that work without wasting too much time in discovering that a plugin is faulty.

Additionally, it is really important to try to detect issues as early as possible to improve Cordova's plugin ecosystem.

If the plugin audit system can detect these issues as early as possible even before the user attempt to use a newly released plugin, we could potentially even help plugin developers by reporting what our system has discovered.

This would also potentially increase the user's experience with Cordova even more.

[&#x1F51D;](#cordova-plugin-audit)

## What are Some of the Future Tasks & Steps

- Include functional local testing. (This may require loading extra plugin on top of the plugin being tested.)

- Introduce a way for developers to include configurations for the plugin's test case. (Some plugins require additional configuration just to work.)

- Release the testing tool to the community and as open-source.

- Include with the testing tool an option where users can share test results back to the audit service. (Increases environment test cases.)

- Implement within the audit service an automatic reporting feature to report discovered issues to the plugin's GitHub Issue Tracker. (Including potentially PR tracking.)

### Possible Future Task within Cordova

- Add to Cordova CLI an interactive feature that runs the audit before `plugin install` and asks if the CLI should continue to install if an issue is present. (This feature may be off by default for CI services or a flag implemented for disabling)

[&#x1F51D;](#cordova-plugin-audit)

## What Might be End-Users Concerns

- **Is the third-party service closed-souced?**

  The API service and Database is closed source.

  Currently, the testing tool that is executed on a macOS, Linux, and Windows for generating the results is the closed source but is planned to be open-source after a few milestones are completed.

  We want users to be able to test locally and potentially provide test results back to the community that gives alternative environment-base results. Users are not obligated to share their results if they choose.

- **Are users restricted to this third-party service?**

  No, if there is another third-party service, users can configure the endpoint to point at that provider.

  One thing to note for perspective third-party service providers, all API request and response structure are governed by Cordova. Cordova should have to conform to support multiple providers.

- **Can users disable the audit?**

  Yes, the audit feature can be disabled.

[&#x1F51D;](#cordova-plugin-audit)
