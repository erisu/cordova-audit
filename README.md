# Cordova Plugin Audit

## What is the Cordova Plugin Audit

Cordova Plugin Audit is a tool and service for testing, gathering, and providing useful information of any given plugin to the end-user.

In essence, the Cordova Plugin Audit is similar to what npm audit does but the main difference is that it does not test for security. The plugin audit system aims to identify issues with the plugin during various stages of its lifecycle. Some stages are not planned to be introduced within the first phase of release.

Below outlines the stages that are tested by the audit system (currently).

[&#x1F51D;](#cordova-plugin-audit)

## How Do End-Users Use This Service in Cordova

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

CLI will display one of the following outcomes below:

#### Plugin Add Fail Example Output

```prompt
...
Usual Install Output
...

audit has detected an issue with the plugin: cordova-plugin-camera@x.y.z
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

CLI will display one of the following outcomes below:

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

## How Do End-Users Access Results Outside of Cordova

Outside of Cordova CLI, users can search for test result with the given test case parameters through the Audit service database (website).

![Audit Service Test Results Website Prototype](../master/example-test-results.png)

[&#x1F51D;](#cordova-plugin-audit)

## What are the End-Users Benefits

The major benefit that an end-user receives from this system is a warning about the newly installed plugin when one or more defects are detected.

There are possibilities that the defects are within the functionality and is not detected within the audit system. The goal though is to try and identify where we can and report to the end-users to reduce time wasted in an attempt to use a bugged plugin.

[&#x1F51D;](#cordova-plugin-audit)

## What Might be End-Users Concerns

- **Is the third-party service closed-souced?**

  The API service and Database is closed source.

  Currently, the testing tool that is executed on a macOS, Linux, and Windows for generating the results is the closed source but is planned to be open-source after a few milestones are completed.

  We want users to be able to test locally and potentially provide test results back to the community that gives alternative environment-base results. Users are not obligated to share their results if they choose.

- **Are users restricted to this third-party service?**

  No, if there is another third-party service, users can configure the endpoint to point at that provider.

  One thing to note for prospective third-party service providers, all API request and response structure are governed by Cordova. Cordova should have to conform to support multiple providers.

- **Can users disable the audit?**

  Yes, the audit feature can be disabled.

[&#x1F51D;](#cordova-plugin-audit)

## Overall Goal

In summary, the end goal is to provide end-users a better user experience so they can focus on developing an app with plugins that work without wasting too much time in discovering that a plugin is faulty.

Additionally, it is really important to try to detect issues as early as possible to improve Cordova's plugin ecosystem.

If the plugin audit system can detect these issues as early as possible even before the user attempt to use a newly released plugin, we could potentially even help plugin developers by reporting what our system has discovered.

This would also potentially increase the user's experience with Cordova even more.

[&#x1F51D;](#cordova-plugin-audit)

## Plugin Audit Technical Details

For more details about the system, please see the [Technical Details](../master/TechnicalDetails.md) document.

[&#x1F51D;](#cordova-plugin-audit)
