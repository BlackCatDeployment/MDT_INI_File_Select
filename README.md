# MDT Wizard: Use an external Customsettings.ini

## Installation

### Edit BDD_Welcome_ENU.xml
This file is located under **\<DS\>\\Scripts** folder

Add "Profiles" pane after "Bootstrap" pane:
```xml
<Pane id="Profiles" title="Profile selection" reference="INI_profiles.xml">
</Pane>
```

Full file content:
```xml
<Wizard>
	<Global>
		<HideNavigation>true</HideNavigation>
	</Global>

 	<Pane id="Bootstrap" title="Processing Bootstrap Settings" reference="WelcomeWiz_Initialize.xml">
	</Pane>

	<Pane id="Profiles" title="Profile selection" reference="INI_profiles.xml">
	</Pane>
	
	<Pane id="Choice" reference="WelcomeWiz_Choice.xml">
		<Condition><![CDATA[ oEnv("SystemDrive") = "X:" and UCase(oEnvironment.Item("SkipBDDWelcome")) <> "YES" ]]></Condition>
	</Pane>

	<Pane id="DeployRoot" title="Deployment Share" reference="WelcomeWiz_DeployRoot.xml">
		<Condition><![CDATA[ UCase(Left(oEnvironment.Item("DeployRoot"),3)) = "X:\" or oEnvironment.Item("DeployRoot") = "" or UCase(oEnvironment.Item("ChooseDeployRoot")) = "YES" ]]></Condition>
	</Pane>
</Wizard>
```

**Note:** If "Profiles" pane is added before "Bootstrap" pane, it may occurs a side-effect if the user click too fast on the Next button. Bootstrap file isn't correctly parsed and the MDT wizard goes on error.

After analysis, it's because the Bootstrap is parsed in the background of Profiles wizard.


### Copy files under ExtraDir
In order to work during the WinPE phase, it's necessary to copy all needed files in the WinPE Extra Directory folder.

* ExtraDir
  * Deploy
    * INI_folder
      * *cs1*.ini
      * *cs2*.ini
    * Scripts
      * INI_Profiles.png
      * INI_profiles.vbs
      * INI_profiles.xml
      
Each time a custom CustomSettings.ini is added or modified, it's necessary to update the Deployment Share.
