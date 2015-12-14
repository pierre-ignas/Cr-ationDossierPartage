#Ajouter un partage utilisateur.
$partage = "Stagiaires"
$dossier = "c:\Utilisateurs"
$desc = "Partage entre tout les stagiaires"
#Droit d'accès aux utilisateurs.
$trustee = ([WMIClass] "Win32_Trustee").CreateInstance()
$trustee.Domain = $Null
$trustee.Name = "Stagiaires"
$Trustee.SID = @(1, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0)   #ID du compte "Stagiaire".
$ace = ([WMIClass] "Win32_ACE").CreateInstance()
$ace.AccessMask = [int][System.Security.AccessControl.FileSystemRights]"FullControl, Synchronize"
#$ace.AccessMask = [int][System.Security.AccessControl.FileSystemRights]"Modify, Synchronize"
#$ace.AccessMask = [int][System.Security.AccessControl.FileSystemRights]"ReadAndExecute, Synchronize"
$ace.AceFlags = 3    #Laisser à 3 pour les partages.
$ace.AceType = 0    #0=accès autoriser, 1=accès interdit.
$ace.Trustee = $trustee
$sd  = ([WMIClass] "Win32_SecurityDescriptor").CreateInstance()  
$sd.ControlFlags = 4
$sd.DACL = $ace
#Partage.
$wmiShares = [WMICLASS] "WIN32_Share"
$wmiShares.Create($dossier,$partage,0,$Null,$desc,$Null,$sd)
#Fin.
