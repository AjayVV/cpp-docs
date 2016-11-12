---
title: "CCommandLineInfo Class | Microsoft Docs"
ms.custom: ""
ms.date: "11/04/2016"
ms.prod: "visual-studio-dev14"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "devlang-cpp"
ms.tgt_pltfrm: ""
ms.topic: "reference"
f1_keywords: 
  - "CCommandLineInfo"
dev_langs: 
  - "C++"
helpviewer_keywords: 
  - "CCommandLineInfo class"
  - "command line, parsing"
  - "parsing, command-line arguments"
  - "argv argument"
  - "startup code, parsing command-line arguments"
  - "application flags [C++]"
ms.assetid: 3e313ddb-0a82-4991-87ac-a27feff4668c
caps.latest.revision: 21
author: "mikeblome"
ms.author: "mblome"
manager: "ghogen"
translation.priority.ht: 
  - "cs-cz"
  - "de-de"
  - "es-es"
  - "fr-fr"
  - "it-it"
  - "ja-jp"
  - "ko-kr"
  - "pl-pl"
  - "pt-br"
  - "ru-ru"
  - "tr-tr"
  - "zh-cn"
  - "zh-tw"
---
# CCommandLineInfo Class
Aids in parsing the command line at application startup.  
  
## Syntax  
  
```  
class CCommandLineInfo : public CObject  
```  
  
## Members  
  
### Public Constructors  
  
|Name|Description|  
|----------|-----------------|  
|[CCommandLineInfo::CCommandLineInfo](#ccommandlineinfo__ccommandlineinfo)|Constructs a default `CCommandLineInfo` object.|  
  
### Public Methods  
  
|Name|Description|  
|----------|-----------------|  
|[CCommandLineInfo::ParseParam](#ccommandlineinfo__parseparam)|Override this callback to parse individual parameters.|  
  
### Public Data Members  
  
|Name|Description|  
|----------|-----------------|  
|[CCommandLineInfo::m_bRunAutomated](#ccommandlineinfo__m_brunautomated)|Indicates the command-line **/Automation** option was found.|  
|[CCommandLineInfo::m_bRunEmbedded](#ccommandlineinfo__m_brunembedded)|Indicates the command-line **/Embedding** option was found.|  
|[CCommandLineInfo::m_bShowSplash](#ccommandlineinfo__m_bshowsplash)|Indicates if a splash screen should be shown.|  
|[CCommandLineInfo::m_nShellCommand](#ccommandlineinfo__m_nshellcommand)|Indicates the shell command to be processed.|  
|[CCommandLineInfo::m_strDriverName](#ccommandlineinfo__m_strdrivername)|Indicates the driver name if the shell command is Print To; otherwise empty.|  
|[CCommandLineInfo::m_strFileName](#ccommandlineinfo__m_strfilename)|Indicates the file name to be opened or printed; empty if the shell command is New or DDE.|  
|[CCommandLineInfo::m_strPortName](#ccommandlineinfo__m_strportname)|Indicates the port name if the shell command is Print To; otherwise empty.|  
|[CCommandLineInfo::m_strPrinterName](#ccommandlineinfo__m_strprintername)|Indicates the printer name if the shell command is Print To; otherwise empty.|  
|[CCommandLineInfo::m_strRestartIdentifier](#ccommandlineinfo__m_strrestartidentifier)|Indicates the unique restart identifier for the restart manager if the restart manager restarted the application.|  
  
## Remarks  
 An MFC application will typically create a local instance of this class in the [InitInstance](../../mfc/reference/cwinapp-class.md#cwinapp__initinstance) function of its application object. This object is then passed to [CWinApp::ParseCommandLine](../../mfc/reference/cwinapp-class.md#cwinapp__parsecommandline), which repeatedly calls [ParseParam](#ccommandlineinfo__parseparam) to fill the `CCommandLineInfo` object. The `CCommandLineInfo` object is then passed to [CWinApp::ProcessShellCommand](../../mfc/reference/cwinapp-class.md#cwinapp__processshellcommand) to handle the command-line arguments and flags.  
  
 You can use this object to encapsulate the following command-line options and parameters:  
  
|Command-line argument|Command executed|  
|----------------------------|----------------------|  
|*app*|New file.|  
|*app* filename|Open file.|  
|*app* **/p** filename|Print file to default printer.|  
|*app* **/pt** filename printer driver port|Print file to the specified printer.|  
|*app* **/dde**|Start up and await DDE command.|  
|*app* **/Automation**|Start up as an OLE automation server.|  
|*app* **/Embedding**|Start up to edit an embedded OLE item.|  
|*app* **/Register**<br /><br /> *app* **/Regserver**|Informs the application to perform any registration tasks.|  
|*app* **/Unregister**<br /><br /> *app* **/Unregserver**|Informs the application to perform any un-registration tasks.|  
  
 Derive a new class from `CCommandLineInfo` to handle other flags and parameter values. Override [ParseParam](#ccommandlineinfo__parseparam) to handle the new flags.  
  
## Inheritance Hierarchy  
 [CObject](../../mfc/reference/cobject-class.md)  
  
 `CCommandLineInfo`  
  
## Requirements  
 **Header:** afxwin.h  
  
##  <a name="ccommandlineinfo__ccommandlineinfo"></a>  CCommandLineInfo::CCommandLineInfo  
 This constructor creates a `CCommandLineInfo` object with default values.  
  
```  
CCommandLineInfo();
```  
  
### Remarks  
 The default is to show the splash screen ( `m_bShowSplash`**=TRUE**) and to execute the New command on the File menu ( `m_nShellCommand`**=NewFile**).  
  
 The application framework calls [ParseParam](#ccommandlineinfo__parseparam) to fill data members of this object.  
  
### Example  
 [!code-cpp[NVC_MFCDocView#54](../../mfc/codesnippet/CPP/ccommandlineinfo-class_1.cpp)]  
  
##  <a name="ccommandlineinfo__m_brunautomated"></a>  CCommandLineInfo::m_bRunAutomated  
 Indicates that the **/Automation** flag was found on the command line.  
  
```  
BOOL m_bRunAutomated;  
```  
  
### Remarks  
 If **TRUE**, this means start up as an OLE automation server.  
  
##  <a name="ccommandlineinfo__m_brunembedded"></a>  CCommandLineInfo::m_bRunEmbedded  
 Indicates that the **/Embedding** flag was found on the command line.  
  
```  
BOOL m_bRunEmbedded;  
```  
  
### Remarks  
 If **TRUE**, this means start up for editing an embedded OLE item.  
  
##  <a name="ccommandlineinfo__m_bshowsplash"></a>  CCommandLineInfo::m_bShowSplash  
 Indicates that the splash screen should be displayed.  
  
```  
BOOL m_bShowSplash;  
```  
  
### Remarks  
 If **TRUE**, this means the splash screen for this application should be displayed during startup. The default implementation of [ParseParam](#ccommandlineinfo__parseparam) sets this data member to **TRUE** if [m_nShellCommand](#ccommandlineinfo__m_nshellcommand) is equal to **CCommandLineInfo::FileNew**.  
  
##  <a name="ccommandlineinfo__m_nshellcommand"></a>  CCommandLineInfo::m_nShellCommand  
 Indicates the shell command for this instance of the application.  
  
```  
m_nShellCommand;  
```  
  
### Remarks  
 The type for this data member is the following enumerated type, which is defined in the `CCommandLineInfo` class.  
  
 `enum{`  
  
 `FileNew,`  
  
 `FileOpen,`  
  
 `FilePrint,`  
  
 `FilePrintTo,`  
  
 `FileDDE,`  
  
 `AppRegister,`  
  
 `AppUnregister,`  
  
 `RestartByRestartManager,`  
  
 `FileNothing = -1`  
  
 `};`  
  
 For a brief description of these values, see the following list.  
  
- `CCommandLineInfo::FileNew` Indicates that no file name was found on the command line.  
  
- `CCommandLineInfo::FileOpen` Indicates that a file name was found on the command line and that none of the following flags were found on the command line: `/p`, `/pt`, `/dde`.  
  
- `CCommandLineInfo::FilePrint` Indicates that the `/p` flag was found on the command line.  
  
- `CCommandLineInfo::FilePrintTo` Indicates that the `/pt` flag was found on the command line.  
  
- `CCommandLineInfo::FileDDE` Indicates that the `/dde` flag was found on the command line.  
  
- `CCommandLineInfo::AppRegister` Indicates that the `/Register` or `/Regserver` flag was found on the command line and the application was asked to register.  
  
- `CCommandLineInfo::AppUnregister` Indicates that the `/Unregister` or `/Unregserver` application was asked to unregister.  
  
- `CCommandLineInfo::RestartByRestartManager` Indicates that the application was restarted by the restart manager.  
  
- `CCommandLineInfo::FileNothing` Turns off the display of a new MDI child window on startup. By design, Application Wizard-generated MDI applications display a new child window on startup. To turn off this feature, an application can use `CCommandLineInfo::FileNothing` as the shell command when it calls [ProcessShellCommand](../../mfc/reference/cwinapp-class.md#cwinapp__processshellcommand). `ProcessShellCommand` is called by the `InitInstance( )` of all `CWinApp` derived classes.  
  
### Example  
 [!code-cpp[NVC_MFCDocView#55](../../mfc/codesnippet/CPP/ccommandlineinfo-class_2.cpp)]  
  
##  <a name="ccommandlineinfo__m_strdrivername"></a>  CCommandLineInfo::m_strDriverName  
 Stores the value of the third non-flag parameter on the command line.  
  
```  
CString m_strDriverName;  
```  
  
### Remarks  
 This parameter is typically the name of the printer driver for a Print To shell command. The default implementation of [ParseParam](#ccommandlineinfo__parseparam) sets this data member only if the **/pt** flag was found on the command line.  
  
##  <a name="ccommandlineinfo__m_strfilename"></a>  CCommandLineInfo::m_strFileName  
 Stores the value of the first non-flag parameter on the command line.  
  
```  
CString m_strFileName;  
```  
  
### Remarks  
 This parameter is typically the name of the file to open.  
  
##  <a name="ccommandlineinfo__m_strportname"></a>  CCommandLineInfo::m_strPortName  
 Stores the value of the fourth non-flag parameter on the command line.  
  
```  
CString m_strPortName;  
```  
  
### Remarks  
 This parameter is typically the name of the printer port for a Print To shell command. The default implementation of [ParseParam](#ccommandlineinfo__parseparam) sets this data member only if the **/pt** flag was found on the command line.  
  
##  <a name="ccommandlineinfo__m_strprintername"></a>  CCommandLineInfo::m_strPrinterName  
 Stores the value of the second non-flag parameter on the command line.  
  
```  
CString m_strPrinterName;  
```  
  
### Remarks  
 This parameter is typically the name of the printer for a Print To shell command. The default implementation of [ParseParam](#ccommandlineinfo__parseparam) sets this data member only if the **/pt** flag was found on the command line.  
  
##  <a name="ccommandlineinfo__m_strrestartidentifier"></a>  CCommandLineInfo::m_strRestartIdentifier  
 The unique restart identifier on the command line.  
  
```  
CString m_strRestartIdentifier;  
```  
  
### Remarks  
 The restart identifier is unique for each instance of the application.  
  
 If the restart manager exits the application and is configured to restart it, the restart manager executes the application from the command line with the restart identifier as an optional parameter. When the restart manager uses the restart identifier, the application can reopen the previously open documents and recover autosaved files.  
  
##  <a name="ccommandlineinfo__parseparam"></a>  CCommandLineInfo::ParseParam  
 The framework calls this function to parse/interpret individual parameters from the command line. The second version differs from the first only in Unicode projects.  
  
```  
virtual void ParseParam(
    const char* pszParam,  
    BOOL bFlag,  
    BOOL bLast);

 
virtual void ParseParam(
    const TCHAR* pszParam,  
    BOOL bFlag,  
    BOOL bLast);
```  
  
### Parameters  
 `pszParam`  
 The parameter or flag.  
  
 *bFlag*  
 Indicates whether `pszParam` is a parameter or a flag.  
  
 `bLast`  
 Indicates if this is the last parameter or flag on the command line.  
  
### Remarks  
 [CWinApp::ParseCommandLine](../../mfc/reference/cwinapp-class.md#cwinapp__parsecommandline) calls `ParseParam` once for each parameter or flag on the command line, passing the argument to `pszParam`. If the first character of the parameter is a ' **-**' or a ' **/**', then it is removed and *bFlag* is set to **TRUE**. When parsing the final parameter, `bLast` is set to **TRUE**.  
  
 The default implementation of this function recognizes the following flags: **/p**, **/pt**, **/dde**, **/Automation**, and **/Embedding**, as shown in the following table:  
  
|Command-line argument|Command executed|  
|----------------------------|----------------------|  
|*app*|New file.|  
|*app* filename|Open file.|  
|*app* **/p** filename|Print file to default printer.|  
|*app* **/pt** filename printer driver port|Print file to the specified printer.|  
|*app* **/dde**|Start up and await DDE command.|  
|*app* **/Automation**|Start up as an OLE automation server.|  
|*app* **/Embedding**|Start up to edit an embedded OLE item.|  
|*app* **/Register**<br /><br /> *app* **/Regserver**|Informs the application to perform any registration tasks.|  
|*app* **/Unregister**<br /><br /> *app* **/Unregserver**|Informs the application to perform any un-registration tasks.|  
  
 This information is stored in [m_bRunAutomated](#ccommandlineinfo__m_brunautomated), [m_bRunEmbedded](#ccommandlineinfo__m_brunembedded), and [m_nShellCommand](#ccommandlineinfo__m_nshellcommand). Flags are marked by either a forward-slash ' **/**' or hyphen ' **-**'.  
  
 The default implementation puts the first non-flag parameter into [m_strFileName](#ccommandlineinfo__m_strfilename). In the case of the **/pt** flag, the default implementation puts the second, third, and fourth non-flag parameters into [m_strPrinterName](#ccommandlineinfo__m_strprintername), [m_strDriverName](#ccommandlineinfo__m_strdrivername), and [m_strPortName](#ccommandlineinfo__m_strportname), respectively.  
  
 The default implementation also sets [m_bShowSplash](#ccommandlineinfo__m_bshowsplash) to **TRUE** only in the case of a new file. In the case of a new file, the user has taken action involving the application itself. In any other case, including opening existing files using the shell, the user action involves the file directly. In a document-centric standpoint, the splash screen does not need to announce the application starting up.  
  
 Override this function in your derived class to handle other flag and parameter values.  
  
## See Also  
 [CObject Class](../../mfc/reference/cobject-class.md)   
 [Hierarchy Chart](../../mfc/hierarchy-chart.md)   
 [CWinApp::ParseCommandLine](../../mfc/reference/cwinapp-class.md#cwinapp__parsecommandline)   
 [CWinApp::ProcessShellCommand](../../mfc/reference/cwinapp-class.md#cwinapp__processshellcommand)
