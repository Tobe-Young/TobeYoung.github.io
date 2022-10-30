# Unity iOS Building related

1. ## Setting Copy resources.
```
using UnityEngine;
using UnityEditor;
using System.IO;
using UnityEditor.Callbacks;
using UnityEditor.iOS.Xcode;
using System.Collections.Generic;
using System.Linq;
 
public class XcodeProjectMod : MonoBehaviour
{
 
    static string[] resourceExts = { ".png", ".jpg", ".jpeg", ".storyboard" };
 
    internal static void CopyAndReplaceDirectory(string srcPath, string dstPath, string[] enableExts)
    {
        if (Directory.Exists(dstPath))
            Directory.Delete(dstPath);
        if (File.Exists(dstPath))
            File.Delete(dstPath);
 
        Directory.CreateDirectory(dstPath);
 
        foreach (var file in Directory.GetFiles(srcPath))
        {
            if (enableExts.Contains(System.IO.Path.GetExtension(file)))
            {
                File.Copy(file, Path.Combine(dstPath, Path.GetFileName(file)));
            }
        }
 
        foreach (var dir in Directory.GetDirectories(srcPath))
        {
            CopyAndReplaceDirectory(dir, Path.Combine(dstPath, Path.GetFileName(dir)), enableExts);
        }
    }
 
    public static void GetDirFileList(string dirPath, ref List<string> dirs, string[] enableExts, string subPathFrom="")
    {
        foreach (string path in Directory.GetFiles(dirPath))
        {
            if (enableExts.Contains(System.IO.Path.GetExtension(path)))
            {
                if(subPathFrom != ""){
                    dirs.Add(path.Substring(path.IndexOf(subPathFrom)));
                }else{
                    dirs.Add(path);
                }
            }
        }
 
        if (Directory.GetDirectories(dirPath).Length > 0)
        {
            foreach (string path in Directory.GetDirectories(dirPath))
            {
                GetDirFileList(path, ref dirs, enableExts, subPathFrom);
            }
        }
 
    }
 
    [PostProcessBuild]
    public static void OnPostprocessBuild(BuildTarget buildTarget, string path)
    {
        if (buildTarget == BuildTarget.iOS)
        {
            string projPath = PBXProject.GetPBXProjectPath(path);
            PBXProject pBXProject = new PBXProject();
 
            pBXProject.ReadFromString(File.ReadAllText(projPath));
            string targetGuid = pBXProject.GetUnityMainTargetGuid();
            string projectGuid = pBXProject.ProjectGuid();
 
            pBXProject.AddBuildProperty(projectGuid, "OTHER_LDFLAGS", "-ObjC -all_load -licucore");
            pBXProject.SetBuildProperty(projectGuid, "ENABLE_BITCODE", "NO");
            List<string> resources = new List<string>();
            CopyAndReplaceDirectory("Assets/Plugins/iOS/resources", Path.Combine(path, "resources"), resourceExts);
            GetDirFileList("Assets/Plugins/iOS/resources", ref resources, resourceExts, "resources");
            foreach (string resource in resources)
            {
                Debug.Log(resource);
                string resourcesBuildPhase = pBXProject.GetResourcesBuildPhaseByTarget(targetGuid);
                string resourcesFilesGuid = pBXProject.AddFile(resource, resource, PBXSourceTree.Source);
                pBXProject.AddFileToBuildSection(targetGuid, resourcesBuildPhase, resourcesFilesGuid);
            }
 
            File.WriteAllText(projPath, pBXProject.WriteToString());
 
        }
    }
 
}
```
2. ## etc...