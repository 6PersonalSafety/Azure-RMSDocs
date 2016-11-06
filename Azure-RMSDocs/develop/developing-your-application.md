﻿---
# required metadata

title: Developing your application | Azure RMS
description: Instructions about how to develop an application using the RMS SDK 2.1.
keywords:
author: bruceperlerms
manager: mbaldwin
ms.date: 11/01/2016
ms.topic: article
ms.prod:
ms.service: information-protection
ms.technology: techgroup-identity
ms.assetid: 396A2C19-3A00-4E9A-9088-198A48B15289
# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: shubhamp
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Developing your application

This topic contains essential guidance on the core aspects of an RMS enabled application and can serve as the foundation of your own application development.

## Introduction

The guidance in this topic is based on the sample application, *IPCHelloWorld*, which will help orient you to the basic concepts and code of a rights-enabled application. The *IPCHelloWorld* project is already configured for the Rights Management Services SDK 2.1.

### Download sample
- Verify that you have registered with the Connect site:
  - To register go to [Connect](http://connect.microsoft.com)
  - Sign in with your Microsoft Account
  - Go to the [Rights Management Connect Site](https://connect.microsoft.com/site1170)
  - Join 
- Download the full *IPCHellowWorld* sample application, as [Webinar_Collateral.zip](https://connect.microsoft.com/site1170/Downloads/DownloadDetails.aspx?DownloadID=42440)

For information about how to configure a new project to use the RMS SDK 2.1, see [Configure Visual Studio](how-to-configure-a-visual-studio-project-to-use-the-ad-rms-sdk-2-0.md).



## Loading MSIPC.dll

Before you can call any RMS SDK 2.1 functions, you need to first call [IpcInitialize](https://msdn.microsoft.com/library/jj127295.aspx) function to load the MSIPC.dll.

        C++
        hr = IpcInitialize();
        if (FAILED(hr)) {
          wprintf(L"Failed to initialize MSIPC. Are you sure the runtime is installed?\n");
          goto exit;
        }

## Enumerating templates

An RMS template defines the policy used to protect the data, i.e. defines the users that are allowed to access the data and their rights. RMS templates are installed on the RMS server.

The following code snip enumerates the available RMS templates from the default RMS server.

      C++
      hr = IpcGetTemplateList(NULL, 0, 0, NULL, NULL, &pcTil);

      if (FAILED(hr)) {
        DisplayError(L"IpcGetTemplateList failed", hr);
        goto exit;
      }

This call will retrieve RMS templates installed on the default server and load the results in the [IPC_TIL](https://msdn.microsoft.com/library/hh535283.aspx) structure pointed by the *pcTil* variable, then display the templates.

      C++
      if (0 == pcTil->cTi) {
        wprintf(L"*** No templates configured for your RMS server ***\n\n");
        wprintf(L"\\------------------------------------------------------\n\n");
        goto exit;
      }

      for (DWORD dw = 0; dw < pcTil->cTi; dw++) {
        wprintf(L"Template #%d:\n", dw);
        wprintf(L"    Name:         %s\n", pcTil->aTi[dw].wszName);
        wprintf(L"    Description:  %s\n", pcTil->aTi[dw].wszDescription);
        wprintf(L"    Issued by:    %s\n", pcTil->aTi[dw].wszIssuerDisplayName);
        wprintf(L"\n");
      }

## Serializing a license

Before you can protect any data, you need to serialize a license and get a content key. The content key is used to encrypt the sensitive data. The serialized license is usually attached to the encrypted data and is used by the consumer of the protected data. The consumer will need to call the [IpcGetKey](https://msdn.microsoft.com/library/hh535263.aspx) function using the serialized license to get the content key for decrypting the content and for getting the policy associated with the content.

For the sake of simplicity use the first RMS template returned by [IpcGetTemplateList](https://msdn.microsoft.com/library/hh535267.aspx) to serialize a license.

Normally, you would use a user interface dialog to allow the user to select the desired template.

      C++
      hr = IpcSerializeLicense((LPCVOID)pcTil->aTi[0].wszID, IPC_SL_TEMPLATE_ID,
        0, NULL, &hContentKey, &pSerializedLicense);

      if (FAILED(hr)) {
        DisplayError(L"IpcSerializeLicense failed", hr);
        goto exit;
      }

After doing this you have the content key, *hContentKey*, and the serialized license, *pSerializedLicense*, that you need to attach to the protected data.


## Protecting data

Now you are ready to encrypt the sensitive data using the [IpcEncrypt](https://msdn.microsoft.com/library/hh535259.aspx) function. First, you need to ask the **IpcEncrypt** function how big the encrypted data is going to be.

      C++
      cbText = (DWORD)(sizeof(WCHAR)*(wcslen(wszText)+1));
      hr = IpcEncrypt(hContentKey, 0, TRUE, (PBYTE)wszText, cbText,
        NULL, 0, &cbEncrypted);

      if (FAILED(hr)) {
        DisplayError(L"IpcEncrypt failed", hr);
        goto exit;
      }

Here *wszText* contains the plain text that you are going to protect. The [IpcEncrypt](https://msdn.microsoft.com/library/hh535259.aspx) function returns the size of the encrypted data in the *cbEncrypted* parameter.

Now allocate memory for the encrypted data.

      C++
      pbEncrypted = (PBYTE)LocalAlloc(LPTR, cbEncrypted);

      if (NULL == pbEncrypted) {
        wprintf(L"Out of memory\n");
        goto exit;
      }

Finally, you can do the actual encryption.

      C++
      hr = IpcEncrypt(hContentKey, 0, TRUE, (PBYTE)wszText, cbText,
        pbEncrypted, cbEncrypted, &cbEncrypted);

      if (FAILED(hr)) {
        DisplayError(L"IpcEncrypt failed", hr);
        goto exit;
      }

After this step you have the encrypted data, *pbEncrypted*, and the serialized license, *pSerializedLicense*, that will be used by consumers to decrypt the data.

## Error handling

Throughout this example application the *DisplayError* function is being used to handle errors.

      C++
      void DisplayError(LPCWSTR wszErrorInfo, HRESULT hrError)
      {
        LPCWSTR wszErrorMessageText = NULL;

        if (SUCCEEDED(IpcGetErrorMessageText(hrError, 0, &wszErrorMessageText))) {
          wprintf(L"%s: 0x%08X (%s)\n", wszErrorInfo, hrError, wszErrorMessageText);
        }
        else {
          wprintf(L"%s: 0x%08X\n", wszErrorInfo, hrError);
        }
      }

The *DisplayError* function uses the [IpcGetErrorMessageText](https://msdn.microsoft.com/library/hh535261.aspx) function to get the error message from the corresponding error code and prints it to the standard output.

## Cleaning up

Before you are done, you also need to release all the allocated resources.

      C++
      if (NULL != pbEncrypted) {
        LocalFree((HLOCAL)pbEncrypted);
      }

      if (NULL != pSerializedLicense) {
        IpcFreeMemory((LPVOID)pSerializedLicense);
      }

      if (NULL != hContentKey) {
        IpcCloseHandle((IPC_HANDLE)hContentKey);
      }

      if (NULL != pcTil) {
        IpcFreeMemory((LPVOID)pcTil);
      }

## Related topics

- [Developer guidance and information](developer-notes.md)
- [IpcEncrypt](https://msdn.microsoft.com/library/hh535259.aspx)
- [IpcGetErrorMessageText](https://msdn.microsoft.com/library/hh535261.aspx)
- [IpcGetKey](https://msdn.microsoft.com/library/hh535263.aspx)
- [IpcGetTemplateList](https://msdn.microsoft.com/library/hh535267.aspx)
- [IpcInitialize](https://msdn.microsoft.com/library/jj127295.aspx)
- [IPC_TIL](https://msdn.microsoft.com/library/hh535283.aspx)
- [Webinar_Collateral.zip](https://connect.microsoft.com/site1170/Downloads/DownloadDetails.aspx?DownloadID=42440)
