---
title: "Permissions"
source: "https://stalw.art/docs/ref/permissions"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/permissions"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Permissions"
---
# Permissions

      

Permissions are identifiers granted to accounts, groups, tenants, roles, and credentials. Each identifier is referenced in JMAP payloads as a plain string.

The first section lists the baseline permissions declared in the permission schema. The second section lists the `sys*` permissions automatically generated for every server object (one category per object, covering `Get` / `Create` / `Update` / `Destroy` / `Query` for objects and `Get` / `Update` for singletons).

## Base permissions

[Section titled “Base permissions”](#base-permissions)

### Admin

[Section titled “Admin”](#admin)

`impersonate`: Act on behalf of another user
`unlimitedRequests`: Bypass request rate limits
`unlimitedUploads`: Bypass upload size and rate limits
`fetchAnyBlob`: Retrieve arbitrary blobs, even those not owned by the account

### Calendar

[Section titled “Calendar”](#calendar)

`calendarAlarmsSend`: Send calendar alarm notifications via email
`calendarSchedulingSend`: Send calendar invitations and updates via email
`calendarSchedulingReceive`: Process incoming calendar invitations and replies

### Dav

[Section titled “Dav”](#dav)

`davSyncCollection`: Synchronize collection changes with client
`davExpandProperty`: Expand properties that reference other resources
`davPrincipalAcl`: Manage access control properties on principals
`davPrincipalList`: List available principals in the system
`davPrincipalMatch`: Match principals based on specified criteria
`davPrincipalSearch`: Search for principals by property values
`davPrincipalSearchPropSet`: Query which properties can be searched on principals

### DavCal

[Section titled “DavCal”](#davcal)

`davCalPropFind`: Retrieve properties of calendar entries
`davCalPropPatch`: Modify properties of calendar entries
`davCalGet`: Download calendar entries
`davCalMkCol`: Create new calendar collections
`davCalDelete`: Remove calendar entries or collections
`davCalPut`: Upload or modify calendar entries
`davCalCopy`: Copy calendar entries to new locations
`davCalMove`: Move calendar entries to new locations
`davCalLock`: Lock calendar entries to prevent concurrent modifications
`davCalAcl`: Manage access control lists for calendar entries
`davCalQuery`: Search for calendar entries matching criteria
`davCalMultiGet`: Retrieve multiple calendar entries in a single request
`davCalFreeBusyQuery`: Query free/busy time information for scheduling

### DavCard

[Section titled “DavCard”](#davcard)

`davCardPropFind`: Retrieve properties of address book entries
`davCardPropPatch`: Modify properties of address book entries
`davCardGet`: Download address book entries
`davCardMkCol`: Create new address book collections
`davCardDelete`: Remove address book entries or collections
`davCardPut`: Upload or modify address book entries
`davCardCopy`: Copy address book entries to new locations
`davCardMove`: Move address book entries to new locations
`davCardLock`: Lock address book entries to prevent concurrent modifications
`davCardAcl`: Manage access control lists for address book entries
`davCardQuery`: Search for address book entries matching criteria
`davCardMultiGet`: Retrieve multiple address book entries in a single request

### DavFile

[Section titled “DavFile”](#davfile)

`davFilePropFind`: Retrieve properties of file resources
`davFilePropPatch`: Modify properties of file resources
`davFileGet`: Download file resources
`davFileMkCol`: Create new file collections or directories
`davFileDelete`: Remove file resources
`davFilePut`: Upload or modify file resources
`davFileCopy`: Copy file resources to new locations
`davFileMove`: Move file resources to new locations
`davFileLock`: Lock file resources to prevent concurrent modifications
`davFileAcl`: Manage access control lists for file resources

### Email

[Section titled “Email”](#email)

`emailSend`: Send emails
`emailReceive`: Receive emails

### Imap

[Section titled “Imap”](#imap)

`imapAuthenticate`: Sign in to the IMAP server
`imapAclGet`: View mailbox access control lists
`imapAclSet`: Modify mailbox access control lists
`imapMyRights`: View own permissions on a mailbox
`imapListRights`: List available permission flags for a mailbox
`imapAppend`: Upload messages to a mailbox
`imapCapability`: Query supported IMAP extensions
`imapId`: Query server identification and version
`imapCopy`: Copy messages between mailboxes
`imapMove`: Move messages between mailboxes
`imapCreate`: Create new mailboxes
`imapDelete`: Delete mailboxes or mark messages for deletion
`imapEnable`: Activate optional IMAP extensions
`imapExpunge`: Permanently remove messages marked for deletion
`imapFetch`: Download message content and metadata
`imapIdle`: Receive real-time notifications for mailbox changes
`imapList`: List available mailboxes and their attributes
`imapLsub`: List mailboxes the user is subscribed to
`imapNamespace`: Query available mailbox namespace prefixes
`imapRename`: Rename or move mailboxes
`imapSearch`: Search messages by criteria
`imapSort`: Retrieve messages in a specified sort order
`imapSelect`: Open a mailbox for read-write access
`imapExamine`: Open a mailbox for read-only access
`imapStatus`: Query mailbox message counts and state
`imapStore`: Set or clear message flags (read, flagged, deleted, etc.)
`imapSubscribe`: Subscribe or unsubscribe to mailboxes
`imapThread`: Group messages into conversation threads

### JmapAddressBook

[Section titled “JmapAddressBook”](#jmapaddressbook)

`jmapAddressBookGet`: Retrieve address book details and properties
`jmapAddressBookChanges`: Track changes to address books since a given state
`jmapAddressBookCreate`: Create new address books
`jmapAddressBookUpdate`: Modify existing address books
`jmapAddressBookDestroy`: Remove address books

### JmapBlob

[Section titled “JmapBlob”](#jmapblob)

`jmapBlobGet`: Download blob data
`jmapBlobCopy`: Copy blobs to another account
`jmapBlobLookup`: Look up blob usage across data types
`jmapBlobUpload`: Upload new blob data

### JmapCalendar

[Section titled “JmapCalendar”](#jmapcalendar)

`jmapCalendarGet`: Retrieve calendar details and properties
`jmapCalendarChanges`: Track changes to calendars since a given state
`jmapCalendarCreate`: Create new calendars
`jmapCalendarUpdate`: Modify existing calendars
`jmapCalendarDestroy`: Remove calendars

### JmapCalendarEvent

[Section titled “JmapCalendarEvent”](#jmapcalendarevent)

`jmapCalendarEventGet`: Retrieve calendar event details
`jmapCalendarEventChanges`: Track changes to calendar events since a given state
`jmapCalendarEventQuery`: Search for calendar events matching criteria
`jmapCalendarEventQueryChanges`: Track changes to calendar event query results
`jmapCalendarEventCreate`: Create new calendar events
`jmapCalendarEventUpdate`: Modify existing calendar events
`jmapCalendarEventDestroy`: Remove calendar events
`jmapCalendarEventCopy`: Copy calendar events to another account
`jmapCalendarEventParse`: Parse raw calendar event data without storing

### JmapCalendarEventNotification

[Section titled “JmapCalendarEventNotification”](#jmapcalendareventnotification)

`jmapCalendarEventNotificationGet`: Retrieve calendar event notification details
`jmapCalendarEventNotificationChanges`: Track changes to calendar event notifications since a given state
`jmapCalendarEventNotificationQuery`: Search for calendar event notifications matching criteria
`jmapCalendarEventNotificationQueryChanges`: Track changes to calendar event notification query results
`jmapCalendarEventNotificationCreate`: Create new calendar event notifications
`jmapCalendarEventNotificationUpdate`: Modify existing calendar event notifications
`jmapCalendarEventNotificationDestroy`: Remove calendar event notifications

### JmapContactCard

[Section titled “JmapContactCard”](#jmapcontactcard)

`jmapContactCardGet`: Retrieve contact card details
`jmapContactCardChanges`: Track changes to contact cards since a given state
`jmapContactCardQuery`: Search for contact cards matching criteria
`jmapContactCardQueryChanges`: Track changes to contact card query results
`jmapContactCardCreate`: Create new contact cards
`jmapContactCardUpdate`: Modify existing contact cards
`jmapContactCardDestroy`: Remove contact cards
`jmapContactCardCopy`: Copy contact cards to another account
`jmapContactCardParse`: Parse raw contact card data without storing

### JmapCore

[Section titled “JmapCore”](#jmapcore)

`jmapCoreEcho`: Echo back input data for testing

### JmapEmail

[Section titled “JmapEmail”](#jmapemail)

`jmapEmailGet`: Retrieve email details and properties
`jmapEmailChanges`: Track changes to emails since a given state
`jmapEmailQuery`: Search for emails matching criteria
`jmapEmailQueryChanges`: Track changes to email query results
`jmapEmailCreate`: Create new emails
`jmapEmailUpdate`: Modify existing emails
`jmapEmailDestroy`: Remove emails
`jmapEmailCopy`: Copy emails to another account
`jmapEmailImport`: Import emails from blob data
`jmapEmailParse`: Parse raw email data without storing

### JmapEmailSubmission

[Section titled “JmapEmailSubmission”](#jmapemailsubmission)

`jmapEmailSubmissionGet`: Retrieve email submission details
`jmapEmailSubmissionChanges`: Track changes to email submissions since a given state
`jmapEmailSubmissionQuery`: Search for email submissions matching criteria
`jmapEmailSubmissionQueryChanges`: Track changes to email submission query results
`jmapEmailSubmissionCreate`: Create new email submissions for sending
`jmapEmailSubmissionUpdate`: Modify existing email submissions
`jmapEmailSubmissionDestroy`: Remove email submissions

### JmapFileNode

[Section titled “JmapFileNode”](#jmapfilenode)

`jmapFileNodeGet`: Retrieve file node details and properties
`jmapFileNodeChanges`: Track changes to file nodes since a given state
`jmapFileNodeQuery`: Search for file nodes matching criteria
`jmapFileNodeQueryChanges`: Track changes to file node query results
`jmapFileNodeCreate`: Create new file nodes
`jmapFileNodeUpdate`: Modify existing file nodes
`jmapFileNodeDestroy`: Remove file nodes
`jmapFileNodeCopy`: Copy file nodes to another account

### JmapIdentity

[Section titled “JmapIdentity”](#jmapidentity)

`jmapIdentityGet`: Retrieve sender identity details
`jmapIdentityChanges`: Track changes to identities since a given state
`jmapIdentityCreate`: Create new sender identities
`jmapIdentityUpdate`: Modify existing sender identities
`jmapIdentityDestroy`: Remove sender identities

### JmapMailbox

[Section titled “JmapMailbox”](#jmapmailbox)

`jmapMailboxGet`: Retrieve mailbox details and properties
`jmapMailboxChanges`: Track changes to mailboxes since a given state
`jmapMailboxQuery`: Search for mailboxes matching criteria
`jmapMailboxQueryChanges`: Track changes to mailbox query results
`jmapMailboxCreate`: Create new mailboxes
`jmapMailboxUpdate`: Modify existing mailboxes
`jmapMailboxDestroy`: Remove mailboxes

### JmapParticipantIdentity

[Section titled “JmapParticipantIdentity”](#jmapparticipantidentity)

`jmapParticipantIdentityGet`: Retrieve participant identity details
`jmapParticipantIdentityChanges`: Track changes to participant identities since a given state
`jmapParticipantIdentityCreate`: Create new participant identities
`jmapParticipantIdentityUpdate`: Modify existing participant identities
`jmapParticipantIdentityDestroy`: Remove participant identities

### JmapPrincipal

[Section titled “JmapPrincipal”](#jmapprincipal)

`jmapPrincipalGet`: Retrieve principal details and properties
`jmapPrincipalQuery`: Search for principals matching criteria
`jmapPrincipalChanges`: Track changes to principals since a given state
`jmapPrincipalQueryChanges`: Track changes to principal query results
`jmapPrincipalGetAvailability`: Query availability information for principals
`jmapPrincipalCreate`: Create new principals
`jmapPrincipalUpdate`: Modify existing principals
`jmapPrincipalDestroy`: Remove principals

### JmapPushSubscription

[Section titled “JmapPushSubscription”](#jmappushsubscription)

`jmapPushSubscriptionGet`: Retrieve push subscription details
`jmapPushSubscriptionCreate`: Create new push subscriptions
`jmapPushSubscriptionUpdate`: Modify existing push subscriptions
`jmapPushSubscriptionDestroy`: Remove push subscriptions

### JmapQuota

[Section titled “JmapQuota”](#jmapquota)

`jmapQuotaGet`: Retrieve quota usage and limits
`jmapQuotaChanges`: Track changes to quotas since a given state
`jmapQuotaQuery`: Search for quotas matching criteria
`jmapQuotaQueryChanges`: Track changes to quota query results

### JmapSearchSnippet

[Section titled “JmapSearchSnippet”](#jmapsearchsnippet)

`jmapSearchSnippetGet`: Retrieve search result snippets for emails

### JmapShareNotification

[Section titled “JmapShareNotification”](#jmapsharenotification)

`jmapShareNotificationGet`: Retrieve share notification details
`jmapShareNotificationChanges`: Track changes to share notifications since a given state
`jmapShareNotificationQuery`: Search for share notifications matching criteria
`jmapShareNotificationQueryChanges`: Track changes to share notification query results
`jmapShareNotificationCreate`: Create new share notifications
`jmapShareNotificationUpdate`: Modify existing share notifications
`jmapShareNotificationDestroy`: Remove share notifications

### JmapSieveScript

[Section titled “JmapSieveScript”](#jmapsievescript)

`jmapSieveScriptGet`: Retrieve Sieve filter script details
`jmapSieveScriptQuery`: Search for Sieve scripts matching criteria
`jmapSieveScriptValidate`: Validate a Sieve script without storing
`jmapSieveScriptCreate`: Create new Sieve filter scripts
`jmapSieveScriptUpdate`: Modify existing Sieve filter scripts
`jmapSieveScriptDestroy`: Remove Sieve filter scripts

### JmapThread

[Section titled “JmapThread”](#jmapthread)

`jmapThreadGet`: Retrieve email thread details
`jmapThreadChanges`: Track changes to threads since a given state

### JmapVacationResponse

[Section titled “JmapVacationResponse”](#jmapvacationresponse)

`jmapVacationResponseGet`: Retrieve vacation response configuration
`jmapVacationResponseCreate`: Create vacation response settings
`jmapVacationResponseUpdate`: Modify vacation response settings
`jmapVacationResponseDestroy`: Remove vacation response settings

### Live

[Section titled “Live”](#live)

`liveTracing`: View real-time server event traces
`liveMetrics`: View real-time server performance metrics
`liveDeliveryTest`: Send test emails to verify delivery configuration

### OAuth

[Section titled “OAuth”](#oauth)

`oAuthClientRegistration`: Register new OAuth client applications
`oAuthClientOverride`: Bypass OAuth client registration requirements

### Pop3

[Section titled “Pop3”](#pop3)

`pop3Authenticate`: Sign in to the POP3 server
`pop3List`: List messages and their sizes
`pop3Uidl`: Retrieve unique message identifiers
`pop3Stat`: Query mailbox message count and total size
`pop3Retr`: Download message content
`pop3Dele`: Mark messages for deletion on disconnect

### Sieve

[Section titled “Sieve”](#sieve)

`sieveAuthenticate`: Sign in to the ManageSieve server
`sieveListScripts`: List uploaded Sieve scripts
`sieveSetActive`: Designate which Sieve script is active
`sieveGetScript`: Download Sieve script content
`sievePutScript`: Upload or replace a Sieve script
`sieveDeleteScript`: Remove a Sieve script
`sieveRenameScript`: Rename a Sieve script
`sieveCheckScript`: Validate Sieve script syntax without saving
`sieveHaveSpace`: Check if quota allows uploading a script of a given size

### User

[Section titled “User”](#user)

`authenticate`: Sign in to the server
`authenticateWithAlias`: Sign in using email alias addresses
`interactAi`: Interact with AI models

## Registry permissions

[Section titled “Registry permissions”](#registry-permissions)

### sysAccount

[Section titled “sysAccount”](#sysaccount)

`sysAccountGet`: Get accounts
`sysAccountCreate`: Create accounts
`sysAccountUpdate`: Update accounts
`sysAccountDestroy`: Destroy accounts
`sysAccountQuery`: Query accounts

### sysAccountPassword

[Section titled “sysAccountPassword”](#sysaccountpassword)

`sysAccountPasswordGet`: Get AccountPassword settings
`sysAccountPasswordUpdate`: Update AccountPassword settings

### sysAccountSettings

[Section titled “sysAccountSettings”](#sysaccountsettings)

`sysAccountSettingsGet`: Get AccountSettings settings
`sysAccountSettingsUpdate`: Update AccountSettings settings

### sysAcmeProvider

[Section titled “sysAcmeProvider”](#sysacmeprovider)

`sysAcmeProviderGet`: Get providers
`sysAcmeProviderCreate`: Create providers
`sysAcmeProviderUpdate`: Update providers
`sysAcmeProviderDestroy`: Destroy providers
`sysAcmeProviderQuery`: Query providers

### sysAction

[Section titled “sysAction”](#sysaction)

`actionReloadSettings`: Action: Reload: Server settings
`actionReloadTlsCertificates`: Action: Reload: TLS certificates
`actionReloadLookupStores`: Action: Reload: Lookup stores
`actionReloadBlockedIps`: Action: Reload: Blocked IPs list
`actionUpdateApps`: Action: Application Management: Update applications
`actionTroubleshootDmarc`: Action: DMARC: Troubleshooting
`actionClassifySpam`: Action: Spam Filter: Classify a message
`actionInvalidateCaches`: Action: Cache: Invalidate all caches
`actionInvalidateNegativeCaches`: Action: Cache: Invalidate negative caches
`actionPauseMtaQueue`: Action: MTA: Pause queue processing
`actionResumeMtaQueue`: Action: MTA: Resume queue processing
`sysActionGet`: Get actions
`sysActionCreate`: Create actions
`sysActionUpdate`: Update actions
`sysActionDestroy`: Destroy actions
`sysActionQuery`: Query actions

### sysAddressBook

[Section titled “sysAddressBook”](#sysaddressbook)

`sysAddressBookGet`: Get AddressBook settings
`sysAddressBookUpdate`: Update AddressBook settings

### sysAiModel

[Section titled “sysAiModel”](#sysaimodel)

`sysAiModelGet`: Get AI models
`sysAiModelCreate`: Create AI models
`sysAiModelUpdate`: Update AI models
`sysAiModelDestroy`: Destroy AI models
`sysAiModelQuery`: Query AI models

### sysAlert

[Section titled “sysAlert”](#sysalert)

`sysAlertGet`: Get alerts
`sysAlertCreate`: Create alerts
`sysAlertUpdate`: Update alerts
`sysAlertDestroy`: Destroy alerts
`sysAlertQuery`: Query alerts

### sysAllowedIp

[Section titled “sysAllowedIp”](#sysallowedip)

`sysAllowedIpGet`: Get addresses
`sysAllowedIpCreate`: Create addresses
`sysAllowedIpUpdate`: Update addresses
`sysAllowedIpDestroy`: Destroy addresses
`sysAllowedIpQuery`: Query addresses

### sysApiKey

[Section titled “sysApiKey”](#sysapikey)

`sysApiKeyGet`: Get API keys
`sysApiKeyCreate`: Create API keys
`sysApiKeyUpdate`: Update API keys
`sysApiKeyDestroy`: Destroy API keys
`sysApiKeyQuery`: Query API keys

### sysAppPassword

[Section titled “sysAppPassword”](#sysapppassword)

`sysAppPasswordGet`: Get App passwords
`sysAppPasswordCreate`: Create App passwords
`sysAppPasswordUpdate`: Update App passwords
`sysAppPasswordDestroy`: Destroy App passwords
`sysAppPasswordQuery`: Query App passwords

### sysApplication

[Section titled “sysApplication”](#sysapplication)

`sysApplicationGet`: Get applications
`sysApplicationCreate`: Create applications
`sysApplicationUpdate`: Update applications
`sysApplicationDestroy`: Destroy applications
`sysApplicationQuery`: Query applications

### sysArchivedItem

[Section titled “sysArchivedItem”](#sysarchiveditem)

`sysArchivedItemGet`: Get archived items
`sysArchivedItemCreate`: Create archived items
`sysArchivedItemUpdate`: Update archived items
`sysArchivedItemDestroy`: Destroy archived items
`sysArchivedItemQuery`: Query archived items

### sysArfExternalReport

[Section titled “sysArfExternalReport”](#sysarfexternalreport)

`sysArfExternalReportGet`: Get reports
`sysArfExternalReportCreate`: Create reports
`sysArfExternalReportUpdate`: Update reports
`sysArfExternalReportDestroy`: Destroy reports
`sysArfExternalReportQuery`: Query reports

### sysAsn

[Section titled “sysAsn”](#sysasn)

`sysAsnGet`: Get Asn settings
`sysAsnUpdate`: Update Asn settings

### sysAuthentication

[Section titled “sysAuthentication”](#sysauthentication)

`sysAuthenticationGet`: Get Authentication settings
`sysAuthenticationUpdate`: Update Authentication settings

### sysBlobStore

[Section titled “sysBlobStore”](#sysblobstore)

`sysBlobStoreGet`: Get BlobStore settings
`sysBlobStoreUpdate`: Update BlobStore settings

### sysBlockedIp

[Section titled “sysBlockedIp”](#sysblockedip)

`sysBlockedIpGet`: Get addresses
`sysBlockedIpCreate`: Create addresses
`sysBlockedIpUpdate`: Update addresses
`sysBlockedIpDestroy`: Destroy addresses
`sysBlockedIpQuery`: Query addresses

### sysBootstrap

[Section titled “sysBootstrap”](#sysbootstrap)

`sysBootstrapGet`: Get Bootstrap settings
`sysBootstrapUpdate`: Update Bootstrap settings

### sysCache

[Section titled “sysCache”](#syscache)

`sysCacheGet`: Get Cache settings
`sysCacheUpdate`: Update Cache settings

### sysCalendar

[Section titled “sysCalendar”](#syscalendar)

`sysCalendarGet`: Get Calendar settings
`sysCalendarUpdate`: Update Calendar settings

### sysCalendarAlarm

[Section titled “sysCalendarAlarm”](#syscalendaralarm)

`sysCalendarAlarmGet`: Get CalendarAlarm settings
`sysCalendarAlarmUpdate`: Update CalendarAlarm settings

### sysCalendarScheduling

[Section titled “sysCalendarScheduling”](#syscalendarscheduling)

`sysCalendarSchedulingGet`: Get CalendarScheduling settings
`sysCalendarSchedulingUpdate`: Update CalendarScheduling settings

### sysCertificate

[Section titled “sysCertificate”](#syscertificate)

`sysCertificateGet`: Get certificates
`sysCertificateCreate`: Create certificates
`sysCertificateUpdate`: Update certificates
`sysCertificateDestroy`: Destroy certificates
`sysCertificateQuery`: Query certificates

### sysClusterNode

[Section titled “sysClusterNode”](#sysclusternode)

`sysClusterNodeGet`: Get cluster nodes
`sysClusterNodeCreate`: Create cluster nodes
`sysClusterNodeUpdate`: Update cluster nodes
`sysClusterNodeDestroy`: Destroy cluster nodes
`sysClusterNodeQuery`: Query cluster nodes

### sysClusterRole

[Section titled “sysClusterRole”](#sysclusterrole)

`sysClusterRoleGet`: Get roles
`sysClusterRoleCreate`: Create roles
`sysClusterRoleUpdate`: Update roles
`sysClusterRoleDestroy`: Destroy roles
`sysClusterRoleQuery`: Query roles

### sysCoordinator

[Section titled “sysCoordinator”](#syscoordinator)

`sysCoordinatorGet`: Get Coordinator settings
`sysCoordinatorUpdate`: Update Coordinator settings

### sysDataRetention

[Section titled “sysDataRetention”](#sysdataretention)

`sysDataRetentionGet`: Get DataRetention settings
`sysDataRetentionUpdate`: Update DataRetention settings

### sysDataStore

[Section titled “sysDataStore”](#sysdatastore)

`sysDataStoreGet`: Get DataStore settings
`sysDataStoreUpdate`: Update DataStore settings

### sysDirectory

[Section titled “sysDirectory”](#sysdirectory)

`sysDirectoryGet`: Get directories
`sysDirectoryCreate`: Create directories
`sysDirectoryUpdate`: Update directories
`sysDirectoryDestroy`: Destroy directories
`sysDirectoryQuery`: Query directories

### sysDkimReportSettings

[Section titled “sysDkimReportSettings”](#sysdkimreportsettings)

`sysDkimReportSettingsGet`: Get DkimReportSettings settings
`sysDkimReportSettingsUpdate`: Update DkimReportSettings settings

### sysDkimSignature

[Section titled “sysDkimSignature”](#sysdkimsignature)

`sysDkimSignatureGet`: Get signatures
`sysDkimSignatureCreate`: Create signatures
`sysDkimSignatureUpdate`: Update signatures
`sysDkimSignatureDestroy`: Destroy signatures
`sysDkimSignatureQuery`: Query signatures

### sysDmarcExternalReport

[Section titled “sysDmarcExternalReport”](#sysdmarcexternalreport)

`sysDmarcExternalReportGet`: Get reports
`sysDmarcExternalReportCreate`: Create reports
`sysDmarcExternalReportUpdate`: Update reports
`sysDmarcExternalReportDestroy`: Destroy reports
`sysDmarcExternalReportQuery`: Query reports

### sysDmarcInternalReport

[Section titled “sysDmarcInternalReport”](#sysdmarcinternalreport)

`sysDmarcInternalReportGet`: Get reports
`sysDmarcInternalReportCreate`: Create reports
`sysDmarcInternalReportUpdate`: Update reports
`sysDmarcInternalReportDestroy`: Destroy reports
`sysDmarcInternalReportQuery`: Query reports

### sysDmarcReportSettings

[Section titled “sysDmarcReportSettings”](#sysdmarcreportsettings)

`sysDmarcReportSettingsGet`: Get DmarcReportSettings settings
`sysDmarcReportSettingsUpdate`: Update DmarcReportSettings settings

### sysDnsResolver

[Section titled “sysDnsResolver”](#sysdnsresolver)

`sysDnsResolverGet`: Get DnsResolver settings
`sysDnsResolverUpdate`: Update DnsResolver settings

### sysDnsServer

[Section titled “sysDnsServer”](#sysdnsserver)

`sysDnsServerGet`: Get DNS servers
`sysDnsServerCreate`: Create DNS servers
`sysDnsServerUpdate`: Update DNS servers
`sysDnsServerDestroy`: Destroy DNS servers
`sysDnsServerQuery`: Query DNS servers

### sysDomain

[Section titled “sysDomain”](#sysdomain)

`sysDomainGet`: Get domains
`sysDomainCreate`: Create domains
`sysDomainUpdate`: Update domains
`sysDomainDestroy`: Destroy domains
`sysDomainQuery`: Query domains

### sysDsnReportSettings

[Section titled “sysDsnReportSettings”](#sysdsnreportsettings)

`sysDsnReportSettingsGet`: Get DsnReportSettings settings
`sysDsnReportSettingsUpdate`: Update DsnReportSettings settings

### sysEmail

[Section titled “sysEmail”](#sysemail)

`sysEmailGet`: Get Email settings
`sysEmailUpdate`: Update Email settings

### sysEnterprise

[Section titled “sysEnterprise”](#sysenterprise)

`sysEnterpriseGet`: Get Enterprise settings
`sysEnterpriseUpdate`: Update Enterprise settings

### sysEventTracingLevel

[Section titled “sysEventTracingLevel”](#syseventtracinglevel)

`sysEventTracingLevelGet`: Get events
`sysEventTracingLevelCreate`: Create events
`sysEventTracingLevelUpdate`: Update events
`sysEventTracingLevelDestroy`: Destroy events
`sysEventTracingLevelQuery`: Query events

### sysFileStorage

[Section titled “sysFileStorage”](#sysfilestorage)

`sysFileStorageGet`: Get FileStorage settings
`sysFileStorageUpdate`: Update FileStorage settings

### sysHttp

[Section titled “sysHttp”](#syshttp)

`sysHttpGet`: Get Http settings
`sysHttpUpdate`: Update Http settings

### sysHttpForm

[Section titled “sysHttpForm”](#syshttpform)

`sysHttpFormGet`: Get HttpForm settings
`sysHttpFormUpdate`: Update HttpForm settings

### sysHttpLookup

[Section titled “sysHttpLookup”](#syshttplookup)

`sysHttpLookupGet`: Get lists
`sysHttpLookupCreate`: Create lists
`sysHttpLookupUpdate`: Update lists
`sysHttpLookupDestroy`: Destroy lists
`sysHttpLookupQuery`: Query lists

### sysImap

[Section titled “sysImap”](#sysimap)

`sysImapGet`: Get Imap settings
`sysImapUpdate`: Update Imap settings

### sysInMemoryStore

[Section titled “sysInMemoryStore”](#sysinmemorystore)

`sysInMemoryStoreGet`: Get InMemoryStore settings
`sysInMemoryStoreUpdate`: Update InMemoryStore settings

### sysJmap

[Section titled “sysJmap”](#sysjmap)

`sysJmapGet`: Get Jmap settings
`sysJmapUpdate`: Update Jmap settings

### sysLog

[Section titled “sysLog”](#syslog)

`sysLogGet`: Get log entries
`sysLogCreate`: Create log entries
`sysLogUpdate`: Update log entries
`sysLogDestroy`: Destroy log entries
`sysLogQuery`: Query log entries

### sysMailingList

[Section titled “sysMailingList”](#sysmailinglist)

`sysMailingListGet`: Get mailing lists
`sysMailingListCreate`: Create mailing lists
`sysMailingListUpdate`: Update mailing lists
`sysMailingListDestroy`: Destroy mailing lists
`sysMailingListQuery`: Query mailing lists

### sysMaskedEmail

[Section titled “sysMaskedEmail”](#sysmaskedemail)

`sysMaskedEmailGet`: Get masked emails
`sysMaskedEmailCreate`: Create masked emails
`sysMaskedEmailUpdate`: Update masked emails
`sysMaskedEmailDestroy`: Destroy masked emails
`sysMaskedEmailQuery`: Query masked emails

### sysMemoryLookupKey

[Section titled “sysMemoryLookupKey”](#sysmemorylookupkey)

`sysMemoryLookupKeyGet`: Get keys
`sysMemoryLookupKeyCreate`: Create keys
`sysMemoryLookupKeyUpdate`: Update keys
`sysMemoryLookupKeyDestroy`: Destroy keys
`sysMemoryLookupKeyQuery`: Query keys

### sysMemoryLookupKeyValue

[Section titled “sysMemoryLookupKeyValue”](#sysmemorylookupkeyvalue)

`sysMemoryLookupKeyValueGet`: Get keys
`sysMemoryLookupKeyValueCreate`: Create keys
`sysMemoryLookupKeyValueUpdate`: Update keys
`sysMemoryLookupKeyValueDestroy`: Destroy keys
`sysMemoryLookupKeyValueQuery`: Query keys

### sysMetric

[Section titled “sysMetric”](#sysmetric)

`sysMetricGet`: Get metrics
`sysMetricCreate`: Create metrics
`sysMetricUpdate`: Update metrics
`sysMetricDestroy`: Destroy metrics
`sysMetricQuery`: Query metrics

### sysMetrics

[Section titled “sysMetrics”](#sysmetrics)

`sysMetricsGet`: Get Metrics settings
`sysMetricsUpdate`: Update Metrics settings

### sysMetricsStore

[Section titled “sysMetricsStore”](#sysmetricsstore)

`sysMetricsStoreGet`: Get MetricsStore settings
`sysMetricsStoreUpdate`: Update MetricsStore settings

### sysMtaConnectionStrategy

[Section titled “sysMtaConnectionStrategy”](#sysmtaconnectionstrategy)

`sysMtaConnectionStrategyGet`: Get strategies
`sysMtaConnectionStrategyCreate`: Create strategies
`sysMtaConnectionStrategyUpdate`: Update strategies
`sysMtaConnectionStrategyDestroy`: Destroy strategies
`sysMtaConnectionStrategyQuery`: Query strategies

### sysMtaDeliverySchedule

[Section titled “sysMtaDeliverySchedule”](#sysmtadeliveryschedule)

`sysMtaDeliveryScheduleGet`: Get schedules
`sysMtaDeliveryScheduleCreate`: Create schedules
`sysMtaDeliveryScheduleUpdate`: Update schedules
`sysMtaDeliveryScheduleDestroy`: Destroy schedules
`sysMtaDeliveryScheduleQuery`: Query schedules

### sysMtaExtensions

[Section titled “sysMtaExtensions”](#sysmtaextensions)

`sysMtaExtensionsGet`: Get MtaExtensions settings
`sysMtaExtensionsUpdate`: Update MtaExtensions settings

### sysMtaHook

[Section titled “sysMtaHook”](#sysmtahook)

`sysMtaHookGet`: Get hooks
`sysMtaHookCreate`: Create hooks
`sysMtaHookUpdate`: Update hooks
`sysMtaHookDestroy`: Destroy hooks
`sysMtaHookQuery`: Query hooks

### sysMtaInboundSession

[Section titled “sysMtaInboundSession”](#sysmtainboundsession)

`sysMtaInboundSessionGet`: Get MtaInboundSession settings
`sysMtaInboundSessionUpdate`: Update MtaInboundSession settings

### sysMtaInboundThrottle

[Section titled “sysMtaInboundThrottle”](#sysmtainboundthrottle)

`sysMtaInboundThrottleGet`: Get throttles
`sysMtaInboundThrottleCreate`: Create throttles
`sysMtaInboundThrottleUpdate`: Update throttles
`sysMtaInboundThrottleDestroy`: Destroy throttles
`sysMtaInboundThrottleQuery`: Query throttles

### sysMtaMilter

[Section titled “sysMtaMilter”](#sysmtamilter)

`sysMtaMilterGet`: Get milters
`sysMtaMilterCreate`: Create milters
`sysMtaMilterUpdate`: Update milters
`sysMtaMilterDestroy`: Destroy milters
`sysMtaMilterQuery`: Query milters

### sysMtaOutboundStrategy

[Section titled “sysMtaOutboundStrategy”](#sysmtaoutboundstrategy)

`sysMtaOutboundStrategyGet`: Get MtaOutboundStrategy settings
`sysMtaOutboundStrategyUpdate`: Update MtaOutboundStrategy settings

### sysMtaOutboundThrottle

[Section titled “sysMtaOutboundThrottle”](#sysmtaoutboundthrottle)

`sysMtaOutboundThrottleGet`: Get throttles
`sysMtaOutboundThrottleCreate`: Create throttles
`sysMtaOutboundThrottleUpdate`: Update throttles
`sysMtaOutboundThrottleDestroy`: Destroy throttles
`sysMtaOutboundThrottleQuery`: Query throttles

### sysMtaQueueQuota

[Section titled “sysMtaQueueQuota”](#sysmtaqueuequota)

`sysMtaQueueQuotaGet`: Get quotas
`sysMtaQueueQuotaCreate`: Create quotas
`sysMtaQueueQuotaUpdate`: Update quotas
`sysMtaQueueQuotaDestroy`: Destroy quotas
`sysMtaQueueQuotaQuery`: Query quotas

### sysMtaRoute

[Section titled “sysMtaRoute”](#sysmtaroute)

`sysMtaRouteGet`: Get routes
`sysMtaRouteCreate`: Create routes
`sysMtaRouteUpdate`: Update routes
`sysMtaRouteDestroy`: Destroy routes
`sysMtaRouteQuery`: Query routes

### sysMtaStageAuth

[Section titled “sysMtaStageAuth”](#sysmtastageauth)

`sysMtaStageAuthGet`: Get MtaStageAuth settings
`sysMtaStageAuthUpdate`: Update MtaStageAuth settings

### sysMtaStageConnect

[Section titled “sysMtaStageConnect”](#sysmtastageconnect)

`sysMtaStageConnectGet`: Get MtaStageConnect settings
`sysMtaStageConnectUpdate`: Update MtaStageConnect settings

### sysMtaStageData

[Section titled “sysMtaStageData”](#sysmtastagedata)

`sysMtaStageDataGet`: Get MtaStageData settings
`sysMtaStageDataUpdate`: Update MtaStageData settings

### sysMtaStageEhlo

[Section titled “sysMtaStageEhlo”](#sysmtastageehlo)

`sysMtaStageEhloGet`: Get MtaStageEhlo settings
`sysMtaStageEhloUpdate`: Update MtaStageEhlo settings

### sysMtaStageMail

[Section titled “sysMtaStageMail”](#sysmtastagemail)

`sysMtaStageMailGet`: Get MtaStageMail settings
`sysMtaStageMailUpdate`: Update MtaStageMail settings

### sysMtaStageRcpt

[Section titled “sysMtaStageRcpt”](#sysmtastagercpt)

`sysMtaStageRcptGet`: Get MtaStageRcpt settings
`sysMtaStageRcptUpdate`: Update MtaStageRcpt settings

### sysMtaSts

[Section titled “sysMtaSts”](#sysmtasts)

`sysMtaStsGet`: Get MtaSts settings
`sysMtaStsUpdate`: Update MtaSts settings

### sysMtaTlsStrategy

[Section titled “sysMtaTlsStrategy”](#sysmtatlsstrategy)

`sysMtaTlsStrategyGet`: Get strategies
`sysMtaTlsStrategyCreate`: Create strategies
`sysMtaTlsStrategyUpdate`: Update strategies
`sysMtaTlsStrategyDestroy`: Destroy strategies
`sysMtaTlsStrategyQuery`: Query strategies

### sysMtaVirtualQueue

[Section titled “sysMtaVirtualQueue”](#sysmtavirtualqueue)

`sysMtaVirtualQueueGet`: Get queues
`sysMtaVirtualQueueCreate`: Create queues
`sysMtaVirtualQueueUpdate`: Update queues
`sysMtaVirtualQueueDestroy`: Destroy queues
`sysMtaVirtualQueueQuery`: Query queues

### sysNetworkListener

[Section titled “sysNetworkListener”](#sysnetworklistener)

`sysNetworkListenerGet`: Get listeners
`sysNetworkListenerCreate`: Create listeners
`sysNetworkListenerUpdate`: Update listeners
`sysNetworkListenerDestroy`: Destroy listeners
`sysNetworkListenerQuery`: Query listeners

### sysOAuthClient

[Section titled “sysOAuthClient”](#sysoauthclient)

`sysOAuthClientGet`: Get OAuth clients
`sysOAuthClientCreate`: Create OAuth clients
`sysOAuthClientUpdate`: Update OAuth clients
`sysOAuthClientDestroy`: Destroy OAuth clients
`sysOAuthClientQuery`: Query OAuth clients

### sysOidcProvider

[Section titled “sysOidcProvider”](#sysoidcprovider)

`sysOidcProviderGet`: Get OidcProvider settings
`sysOidcProviderUpdate`: Update OidcProvider settings

### sysPublicKey

[Section titled “sysPublicKey”](#syspublickey)

`sysPublicKeyGet`: Get public keys
`sysPublicKeyCreate`: Create public keys
`sysPublicKeyUpdate`: Update public keys
`sysPublicKeyDestroy`: Destroy public keys
`sysPublicKeyQuery`: Query public keys

### sysQueuedMessage

[Section titled “sysQueuedMessage”](#sysqueuedmessage)

`sysQueuedMessageGet`: Get messages
`sysQueuedMessageCreate`: Create messages
`sysQueuedMessageUpdate`: Update messages
`sysQueuedMessageDestroy`: Destroy messages
`sysQueuedMessageQuery`: Query messages

### sysReportSettings

[Section titled “sysReportSettings”](#sysreportsettings)

`sysReportSettingsGet`: Get ReportSettings settings
`sysReportSettingsUpdate`: Update ReportSettings settings

### sysRole

[Section titled “sysRole”](#sysrole)

`sysRoleGet`: Get roles
`sysRoleCreate`: Create roles
`sysRoleUpdate`: Update roles
`sysRoleDestroy`: Destroy roles
`sysRoleQuery`: Query roles

### sysSearch

[Section titled “sysSearch”](#syssearch)

`sysSearchGet`: Get Search settings
`sysSearchUpdate`: Update Search settings

### sysSearchStore

[Section titled “sysSearchStore”](#syssearchstore)

`sysSearchStoreGet`: Get SearchStore settings
`sysSearchStoreUpdate`: Update SearchStore settings

### sysSecurity

[Section titled “sysSecurity”](#syssecurity)

`sysSecurityGet`: Get Security settings
`sysSecurityUpdate`: Update Security settings

### sysSenderAuth

[Section titled “sysSenderAuth”](#syssenderauth)

`sysSenderAuthGet`: Get SenderAuth settings
`sysSenderAuthUpdate`: Update SenderAuth settings

### sysSharing

[Section titled “sysSharing”](#syssharing)

`sysSharingGet`: Get Sharing settings
`sysSharingUpdate`: Update Sharing settings

### sysSieveSystemInterpreter

[Section titled “sysSieveSystemInterpreter”](#syssievesysteminterpreter)

`sysSieveSystemInterpreterGet`: Get SieveSystemInterpreter settings
`sysSieveSystemInterpreterUpdate`: Update SieveSystemInterpreter settings

### sysSieveSystemScript

[Section titled “sysSieveSystemScript”](#syssievesystemscript)

`sysSieveSystemScriptGet`: Get scripts
`sysSieveSystemScriptCreate`: Create scripts
`sysSieveSystemScriptUpdate`: Update scripts
`sysSieveSystemScriptDestroy`: Destroy scripts
`sysSieveSystemScriptQuery`: Query scripts

### sysSieveUserInterpreter

[Section titled “sysSieveUserInterpreter”](#syssieveuserinterpreter)

`sysSieveUserInterpreterGet`: Get SieveUserInterpreter settings
`sysSieveUserInterpreterUpdate`: Update SieveUserInterpreter settings

### sysSieveUserScript

[Section titled “sysSieveUserScript”](#syssieveuserscript)

`sysSieveUserScriptGet`: Get scripts
`sysSieveUserScriptCreate`: Create scripts
`sysSieveUserScriptUpdate`: Update scripts
`sysSieveUserScriptDestroy`: Destroy scripts
`sysSieveUserScriptQuery`: Query scripts

### sysSpamClassifier

[Section titled “sysSpamClassifier”](#sysspamclassifier)

`sysSpamClassifierGet`: Get SpamClassifier settings
`sysSpamClassifierUpdate`: Update SpamClassifier settings

### sysSpamDnsblServer

[Section titled “sysSpamDnsblServer”](#sysspamdnsblserver)

`sysSpamDnsblServerGet`: Get servers
`sysSpamDnsblServerCreate`: Create servers
`sysSpamDnsblServerUpdate`: Update servers
`sysSpamDnsblServerDestroy`: Destroy servers
`sysSpamDnsblServerQuery`: Query servers

### sysSpamDnsblSettings

[Section titled “sysSpamDnsblSettings”](#sysspamdnsblsettings)

`sysSpamDnsblSettingsGet`: Get SpamDnsblSettings settings
`sysSpamDnsblSettingsUpdate`: Update SpamDnsblSettings settings

### sysSpamFileExtension

[Section titled “sysSpamFileExtension”](#sysspamfileextension)

`sysSpamFileExtensionGet`: Get extensions
`sysSpamFileExtensionCreate`: Create extensions
`sysSpamFileExtensionUpdate`: Update extensions
`sysSpamFileExtensionDestroy`: Destroy extensions
`sysSpamFileExtensionQuery`: Query extensions

### sysSpamLlm

[Section titled “sysSpamLlm”](#sysspamllm)

`sysSpamLlmGet`: Get SpamLlm settings
`sysSpamLlmUpdate`: Update SpamLlm settings

### sysSpamPyzor

[Section titled “sysSpamPyzor”](#sysspampyzor)

`sysSpamPyzorGet`: Get SpamPyzor settings
`sysSpamPyzorUpdate`: Update SpamPyzor settings

### sysSpamRule

[Section titled “sysSpamRule”](#sysspamrule)

`sysSpamRuleGet`: Get rules
`sysSpamRuleCreate`: Create rules
`sysSpamRuleUpdate`: Update rules
`sysSpamRuleDestroy`: Destroy rules
`sysSpamRuleQuery`: Query rules

### sysSpamSettings

[Section titled “sysSpamSettings”](#sysspamsettings)

`sysSpamSettingsGet`: Get SpamSettings settings
`sysSpamSettingsUpdate`: Update SpamSettings settings

### sysSpamTag

[Section titled “sysSpamTag”](#sysspamtag)

`sysSpamTagGet`: Get tags
`sysSpamTagCreate`: Create tags
`sysSpamTagUpdate`: Update tags
`sysSpamTagDestroy`: Destroy tags
`sysSpamTagQuery`: Query tags

### sysSpamTrainingSample

[Section titled “sysSpamTrainingSample”](#sysspamtrainingsample)

`sysSpamTrainingSampleGet`: Get samples
`sysSpamTrainingSampleCreate`: Create samples
`sysSpamTrainingSampleUpdate`: Update samples
`sysSpamTrainingSampleDestroy`: Destroy samples
`sysSpamTrainingSampleQuery`: Query samples

### sysSpfReportSettings

[Section titled “sysSpfReportSettings”](#sysspfreportsettings)

`sysSpfReportSettingsGet`: Get SpfReportSettings settings
`sysSpfReportSettingsUpdate`: Update SpfReportSettings settings

### sysStoreLookup

[Section titled “sysStoreLookup”](#sysstorelookup)

`sysStoreLookupGet`: Get lookups
`sysStoreLookupCreate`: Create lookups
`sysStoreLookupUpdate`: Update lookups
`sysStoreLookupDestroy`: Destroy lookups
`sysStoreLookupQuery`: Query lookups

### sysSystemSettings

[Section titled “sysSystemSettings”](#syssystemsettings)

`sysSystemSettingsGet`: Get SystemSettings settings
`sysSystemSettingsUpdate`: Update SystemSettings settings

### sysTask

[Section titled “sysTask”](#systask)

`taskIndexDocument`: Task: Index document
`taskUnindexDocument`: Task: Unindex document
`taskIndexTrace`: Task: Index telemetry trace
`taskCalendarAlarmEmail`: Task: Calendar alarm e-mail
`taskCalendarAlarmNotification`: Task: Calendar alarm notification
`taskCalendarItipMessage`: Task: Calendar iTIP message
`taskMergeThreads`: Task: Merge email threads
`taskDmarcReport`: Task: Send DMARC report to remote server
`taskTlsReport`: Task: Send TLS report to remote server
`taskRestoreArchivedItem`: Task: Restore archived item
`taskDestroyAccount`: Task: Destroy account and all associated data
`taskAccountMaintenance`: Task: Perform account maintenance operations
`taskTenantMaintenance`: Task: Perform tenant maintenance operations
`taskStoreMaintenance`: Task: Perform store maintenance operations
`taskSpamFilterMaintenance`: Task: Perform spam filter maintenance operations
`taskAcmeRenewal`: Task: Perform ACME certificate renewal for a domain
`taskDkimManagement`: Task: Perform DKIM key rotation for a domain
`taskDnsManagement`: Task: Perform DNS management for a domain
`sysTaskGet`: Get tasks
`sysTaskCreate`: Create tasks
`sysTaskUpdate`: Update tasks
`sysTaskDestroy`: Destroy tasks
`sysTaskQuery`: Query tasks

### sysTaskManager

[Section titled “sysTaskManager”](#systaskmanager)

`sysTaskManagerGet`: Get TaskManager settings
`sysTaskManagerUpdate`: Update TaskManager settings

### sysTenant

[Section titled “sysTenant”](#systenant)

`sysTenantGet`: Get tenants
`sysTenantCreate`: Create tenants
`sysTenantUpdate`: Update tenants
`sysTenantDestroy`: Destroy tenants
`sysTenantQuery`: Query tenants

### sysTlsExternalReport

[Section titled “sysTlsExternalReport”](#systlsexternalreport)

`sysTlsExternalReportGet`: Get reports
`sysTlsExternalReportCreate`: Create reports
`sysTlsExternalReportUpdate`: Update reports
`sysTlsExternalReportDestroy`: Destroy reports
`sysTlsExternalReportQuery`: Query reports

### sysTlsInternalReport

[Section titled “sysTlsInternalReport”](#systlsinternalreport)

`sysTlsInternalReportGet`: Get reports
`sysTlsInternalReportCreate`: Create reports
`sysTlsInternalReportUpdate`: Update reports
`sysTlsInternalReportDestroy`: Destroy reports
`sysTlsInternalReportQuery`: Query reports

### sysTlsReportSettings

[Section titled “sysTlsReportSettings”](#systlsreportsettings)

`sysTlsReportSettingsGet`: Get TlsReportSettings settings
`sysTlsReportSettingsUpdate`: Update TlsReportSettings settings

### sysTrace

[Section titled “sysTrace”](#systrace)

`sysTraceGet`: Get traces
`sysTraceCreate`: Create traces
`sysTraceUpdate`: Update traces
`sysTraceDestroy`: Destroy traces
`sysTraceQuery`: Query traces

### sysTracer

[Section titled “sysTracer”](#systracer)

`sysTracerGet`: Get tracers
`sysTracerCreate`: Create tracers
`sysTracerUpdate`: Update tracers
`sysTracerDestroy`: Destroy tracers
`sysTracerQuery`: Query tracers

### sysTracingStore

[Section titled “sysTracingStore”](#systracingstore)

`sysTracingStoreGet`: Get TracingStore settings
`sysTracingStoreUpdate`: Update TracingStore settings

### sysWebDav

[Section titled “sysWebDav”](#syswebdav)

`sysWebDavGet`: Get WebDav settings
`sysWebDavUpdate`: Update WebDav settings

### sysWebHook

[Section titled “sysWebHook”](#syswebhook)

`sysWebHookGet`: Get webhooks
`sysWebHookCreate`: Create webhooks
`sysWebHookUpdate`: Update webhooks
`sysWebHookDestroy`: Destroy webhooks
`sysWebHookQuery`: Query webhooks