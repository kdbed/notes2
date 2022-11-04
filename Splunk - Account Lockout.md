

Monitoring [[Active Directory]] account lockouts with [[splunk]]

```text
index=wineventlog Account_Name=<<accountNameHere>>>
(EventCode=4740 OR EventCode=4625 OR EventCode=644 OR EventCode=529 OR EventCode=675 OR EventCode=676 OR EventCode=681 OR EventCode=4771 OR EventCode=4770 OR EventCode=4768 OR EventCode=4776 OR EventCode=4777 OR EventCode=4725 OR EventCode=4723 OR EventCode=4724 OR EventCode=4767 OR EventCode=4800 OR EventCode=4801)
| eval Account0=mvindex(Account_Name,0) | eval Account1=mvindex(Account_Name,1)
| eval Account=case(EventCode==4624,Account1, EventCode==4625,Account1, EventCode==4648,Account1, EventCode==4722,Account1, EventCode==4723,Account1, EventCode==4724,Account1, EventCode==4725,Account1, EventCode==4738,Account1, EventCode==4740,Account1, EventCode==4767,Account1, EventCode==4768,Account0, EventCode==4769,Account0, EventCode==4771,Account0, EventCode==4770,Account0, EventCode==5140,Account0, EventCode==4778,Account0, EventCode==4779,Account0, EventCode==4800,Account0, EventCode==4801,Account0) | fillnull Value="-" Account
| eval ActionBy=case(EventCode==4725,Account0, EventCode==4722,src_user, EventCode==4767,src_user, EventCode==4723,src_user, EventCode==4724,src_user, EventCode==4738,src_user, EventCode==4794,src_user)
| eval Time=strftime(_time, "%m/%d/%y %H:%M:%S") | sort -_time
| eval Caller_Machine=if(Caller_Machine_Name!= NULL, Caller_Machine_Name, Caller_Computer_Name) | fillnull Value="-" Caller_Machine
| rex field=Process_Name "(?P<Process_Name>[^\\\]+)$" | fillnull Value="-" Process_Name
| rex field=Caller_Process_Name "(?P<Caller_Process_Name>[^\\\]+)$" | fillnull Value="-" Caller_Process_Name
| rename "Authentication_Package" as "Auth_Package", "Source_Network_Address" as "Src_Netw_Addr"
| replace "MICROSOFT_AUTHENTICATION_PACKAGE_V1_0" with "MsAuthPkgV1_0" in Auth_Package
| replace "Microsoft Unified Security Protocol Provider" with "MsUnifiedSecProt" in Auth_Package
| eval EventCode=case(EventCode==4740, "4740 Locked", EventCode==4625, "4625 Logon Failed", EventCode==644, "644 Locked", EventCode==529, "529 Logon Failed", EventCode==4768, "4768 Kerb TGT Req", EventCode==4771, "4771 Kerb Pre-Auth Failed", EventCode==4770, "4770 Kerb Svc Tkt Renewed", EventCode==4624, "4624 Logon OK", EventCode==4648, "4648 Logon Attempt Explicit Creds (ie. Task/RunAs)", EventCode==4767, "4767 Unlocked", EventCode==12294, "12294 Potential attack against Administrator", EventCode==4794, "4794 DSRM Admin PW Set Attempt", EventCode==4725, "4725 Disabled", EventCode==4722, "4722 Enabled", EventCode==4723, "4723 PW change attempt",EventCode==4724, "4724 PW reset attempt", EventCode==4738, "4738 Object changed", EventCode==4720, "4720 Created", EventCode==4726, "4726 Deleted", EventCode==4778, "4778 Session Reconnect", EventCode==4779, "4779 Session Disconnect", EventCode==4800, "4800 Wks Locked", EventCode==4801, "4801 Wks Unlocked", 1=1, EventCode)
| eval Logon_Type=case(Logon_Type==2, "2 Interactive", Logon_Type==3, "3 Network", Logon_Type==4, "4 Batch", Logon_Type==5, "5 Service", Logon_Type==7, "7 Unlock", Logon_Type==8, "8 NetworkClearText", Logon_Type==9, "9 NewCredentials", Logon_Type==10, "10 RemoteInteractive", Logon_Type==11, "11 CachedInteractive", 1=1, Logon_Type)
| table Time, Account, EventCode, ActionBy, Caller_Machine, Failure_Reason, Client_Address, Logon_Type, Logon_Process, Auth_Package, Caller_Process_Name, Process_Name, ComputerName, Workstation_Name, Src_Netw_Addr

```