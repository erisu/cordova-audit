# Cordova Plugin Audit - Technical Details

- [Cordova Plugin Audit - Technical Details](#cordova-plugin-audit---technical-details)
  - [What does the Cordova Plugin Audit Test](#what-does-the-cordova-plugin-audit-test)
    - [Additional Test Notes](#additional-test-notes)
  - [How Much Data is Available](#how-much-data-is-available)
  - [How is a Test Triggered](#how-is-a-test-triggered)
  - [How Does The Cordova Plugin Audit Integrate with Cordova](#how-does-the-cordova-plugin-audit-integrate-with-cordova)
  - [What are Some of the Future Tasks & Steps](#what-are-some-of-the-future-tasks--steps)
    - [Possible Future Task within Cordova](#possible-future-task-within-cordova)

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

## What are Some of the Future Tasks & Steps

- Include functional local testing. (This may require loading extra plugin on top of the plugin being tested.)

- Introduce a way for developers to include configurations for the plugin's test case. (Some plugins require additional configuration just to work.)

- Release the testing tool to the community and as open-source.

- Include with the testing tool an option where users can share test results back to the audit service. (Increases environment test cases.)

- Implement within the audit service an automatic reporting feature to report discovered issues to the plugin's GitHub Issue Tracker. (Including potentially PR tracking.)

### Possible Future Task within Cordova

- Add to Cordova CLI an interactive feature that runs the audit before `plugin install` and asks if the CLI should continue to install if an issue is present. (This feature may be off by default for CI services or a flag implemented for disabling)

[&#x1F51D;](#cordova-plugin-audit)

