# Unity ML框架集成

```
    public static void OnPostProcessBuild(BuildTarget buildTarget, string path)
    {
        if(buildTarget != BuildTarget.iOS)
        {
            return;
        }

        string projPath = path + "/Unity-iPhone.xcodeproj/project.pbxproj";
        
        var project = new PBXProject();
        project.ReadFromFile(projPath);
#if UNITY_2019_3_OR_NEWER
        var unityFrameworkGuid  =  project.GetUnityFrameworkTargetGuid();
        var targetGUID = unityFrameworkGuid;
        project.AddBuildProperty(unityFrameworkGuid, "DEFINES_MODULE", "YES");

        var moduleFile = path + "/UnityFramework/UnityFramework.modulemap";
        if (!File.Exists(moduleFile))
        {
            FileUtil.CopyFileOrDirectory("Assets/Plugins/iOS/UnityFramework.modulemap", moduleFile);
            project.AddFile(moduleFile, "UnityFramework/UnityFramework.modulemap");
            project.AddBuildProperty(unityFrameworkGuid, "MODULEMAP_FILE", "$(SRCROOT)/UnityFramework/UnityFramework.modulemap");
        }

        // Headers
        string unityInterfaceGuid = project.FindFileGuidByProjectPath("Classes/Unity/UnityInterface.h");
        project.AddPublicHeaderToBuild(unityFrameworkGuid, unityInterfaceGuid);
        string unityForwardDeclsGuid = project.FindFileGuidByProjectPath("Classes/Unity/UnityForwardDecls.h");
        project.AddPublicHeaderToBuild(unityFrameworkGuid, unityForwardDeclsGuid);
        string unityRenderingGuid = project.FindFileGuidByProjectPath("Classes/Unity/UnityRendering.h");
        project.AddPublicHeaderToBuild(unityFrameworkGuid, unityRenderingGuid);
        string unitySharedDeclsGuid = project.FindFileGuidByProjectPath("Classes/Unity/UnitySharedDecls.h");
        project.AddPublicHeaderToBuild(unityFrameworkGuid, unitySharedDeclsGuid);
#else
         var targetGUID = proj.TargetGuidByName("Unity-iPhone");
         project.SetBuildProperty(targetGUID, "SWIFT_OBJC_BRIDGING_HEADER", "Libraries/Plugins/ML/iOS/HandDetector/Native/HandDetector.h");
#endif
        string presenceMotionPath = "Plugins/ML/iOS";
        //set xcode proj properties
        project.AddBuildProperty(targetGUID, "SWIFT_VERSION", "5.0");
        project.SetBuildProperty(targetGUID, "SWIFT_OBJC_INTERFACE_HEADER_NAME","UnityFramework-Swift.h");
        project.SetBuildProperty(targetGUID, "COREML_CODEGEN_LANGUAGE", "Swift");
        project.SetBuildProperty(targetGUID, "ENABLE_BITCODE", "false");

        //add hand model to xcode proj build phase.
        var buildPhaseGUID = project.AddSourcesBuildPhase(targetGUID);
        var handModelPath = Application.dataPath + "/../MLModels/HandModel.mlmodel";
        var fileGUID = project.AddFile(handModelPath, "/HandModel.mlmodel");
        project.AddFileToBuildSection(targetGUID, buildPhaseGUID, fileGUID);
}
```