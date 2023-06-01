# Bug Fix for Medal DDEX Recipient receiving 0 Byte files
2023-05-26

### Requirements
DDEX recipient, "Medal" should receive release related data in tact; however, the recipient is receiving 0 sized files
from Monstercat currently

### Previous Findings
1. Batch complete files are received without any contents
2. Resource files are received without any contents
3. Mead files are received without any contents

### Researched potential issues (CHAT-GPT generated)
#### Question 1: what is the common cause sftp server receiving 0Byte files
1. File permissions: The source file may have incorrect permissions that prevent the SFTP server from reading its contents. Make sure the file permissions are set correctly to allow the server to access and read the file.

2. File transfer interruption: The file transfer might be getting interrupted or terminated before it completes, resulting in an empty file being received. This can happen due to network connectivity issues, server timeouts, or client-side problems. Check for any disruptions or errors during the file transfer process.

3. File encoding issues: If the file being transferred has an encoding mismatch between the source and destination systems, it can result in empty files. Ensure that the encoding settings are consistent on both the source and destination systems.

4. Disk space limitations: Insufficient disk space on the server can cause the file transfer process to fail and result in 0-byte files. Check the available disk space on the server and make sure it has enough capacity to accommodate the incoming files.

5. Firewall or security restrictions: Firewalls or security configurations on either the client or server side may be blocking or restricting the file transfer process. Verify the firewall settings and ensure that the necessary ports and protocols are open for SFTP communication.

6. Software or configuration issues: Bugs or misconfigurations in the SFTP server software or its configuration files can lead to empty file transfers. Check the server logs for any error messages or warnings that may provide insights into the underlying issue.

#### Question 2: is there any other possible causes?

7. Incorrect file path: The file path specified in the SFTP client may be incorrect, leading to the server receiving an empty file. Double-check the file path and ensure it points to the correct location and file name.

8. File transfer mode: The file transfer mode may be set incorrectly. SFTP supports two transfer modes: binary and ASCII. If the mode is set to ASCII for a binary file or vice versa, it can result in 0-byte files being received. Verify that the transfer mode is correctly configured.

9. Client-side issues: The issue may lie with the SFTP client used to initiate the file transfer. It could be a bug or a misconfiguration on the client side that is causing the empty file transfers. Try using a different SFTP client or updating the existing client to see if the problem persists.

10. Network or connectivity problems: Network issues, such as packet loss or unstable connections, can lead to incomplete file transfers and result in 0-byte files. Check the network stability and ensure that there are no interruptions during the transfer process.

11. Server-side limitations: The SFTP server might have certain limitations or restrictions that prevent the successful transfer of files. For example, there could be a restriction on file size or certain file types. Review the server settings and configurations to ensure they align with the requirements of the file being transferred.

12. Data corruption: It's possible that the file itself is corrupt or incomplete on the source system. Check the integrity of the source file to ensure it is complete and valid before initiating the transfer.

### Approaches
1. A test DDEX recipient with delivery mechanism of `SFTP with key` has been created as node application
2. [Mock Distributor Repo](https://github.com/yongjohnlee80/test-sftp-server)
3. Create a test ddex recipient manually or with SQL script
```sql
INSERT INTO ddex_distributor (dpid, title, receiving_name, config, auto_delivery, auto_delivery_interval, auto_delivery_unit, label_id, validate_uploads) 
VALUES (
        'PADPIDB2016010701A', 
        'John QA Distributor', 
        'John QA Distributor', 
        '{"Deals": [{"DealTerms": {"Usage": [{"UseType": "AsPerContract"}], "TakeDown": null, "TerritoryCode": [], "ValidityPeriod": {"EndDate": "", "StartDate": ""}, "CommercialModelType": "AsPerContract", "ExcludedTerritoryCode": []}}], "OutputOpts": {"UPCFolders": true, "IncludeWriterShares": false, "DisplayArtistNameMatch": "", "IncludeUpdateIndicator": false, "DisplayArtistNameOverride": ""}, "UploadOpts": {"Addr": "127.0.0.1:11115", "Region": "", "Password": "", "Username": "tester", "RemoteRoot": "files", "DeliveryMechanism": "SFTPWithKey"}}'
        false, 24, 'seconds', 'f25fbe4e-3449-4804-bfff-2a01cf6a77fc', true);
```
4. Create a test track and release containing the track, encode them, and approve them
5. Send the release to the test mock distributor

### New Findings
1. The test distributor has recieved all files correctly with contents populated
2. Media (WAV, png) files were all uploaded correctly as well.
3. More testing is required to replicate the Medal DDEX recipient's problem following the potential issues identified in the above.
4. The following are some of the files uploaded.
#### batch complete xml file
```xml
 <ern-c-ftp:ManifestMessage xmlns:ern-c-ftp="http://ddex.net/xml/ern-c-sftp/16" xmlns:xs="http://www.w3.org/2001/XMLSchema-instance" xs:schemaLocation="http://ddex.net/xml/ern-c-sftp/16 http://ddex.net/xml/ern-c-sftp/16/ern-choreography-sftp.xsd" MessageVersionId="ern-c-sftp/16">
   <MessageHeader>
     <MessageSender>
       <PartyId>PADPIDA20145670097</PartyId>
       <TradingName>Monster</TradingName>
     </MessageSender>
     <MessageRecipient>
       <PartyId>PADPIDA20230301052</PartyId>
     </MessageRecipient>
     <MessageCreatedDateTime>2023-05-26T16:05:57-07:00</MessageCreatedDateTime>
   </MessageHeader>
   <IsTestFlag>false</IsTestFlag>
   <RootDirectory>N_20230526040555041</RootDirectory>
   <NumberOfMessages>1</NumberOfMessages>
   <MessageInBatch>
     <MessageType>NewReleaseMessage</MessageType>
     <MessageId>PADPIDA20145670097-A10443ZR0WW1FSAKBM-2023-05-26-04:05:55</MessageId>
     <URL>./243352321142/A10443ZR0WW1FSAKBM.xml</URL>
     <IncludedReleaseId>
       <GRid>A10443ZR0WW1FSAKBM</GRid>
       <ICPN>243352321142</ICPN>
     </IncludedReleaseId>
     <DeliveryType>NewReleaseDelivery</DeliveryType>
     <ProductType>AudioProduct</ProductType>
   </MessageInBatch>
 </ern-c-ftp:ManifestMessage>
```
#### resource files
```xml
 <ern:NewReleaseMessage xmlns:ern="http://ddex.net/xml/ern/382" xmlns:xs="http://www.w3.org/2001/XMLSchema-instance" xs:schemaLocation="http://ddex.net/xml/ern/382 http://ddex.net/xml/ern/382/release-notification.xsd" MessageSchemaVersionId="ern/382" LanguageAndScriptCode="en">
   <MessageHeader>
     <MessageThreadId>2023-05-26-04:05:55</MessageThreadId>
     <MessageId>PADPIDA20145670097-A10443ZR0WW1FSAKBM-2023-05-26-04:05:55</MessageId>
     <MessageSender>
       <PartyId>PADPIDA20145670097</PartyId>
       <PartyName>
         <FullName>Monster</FullName>
       </PartyName>
     </MessageSender>
     <MessageRecipient>
       <PartyId>PADPIDA20230301052</PartyId>
       <PartyName>
         <FullName>Medal</FullName>
       </PartyName>
     </MessageRecipient>
     <MessageCreatedDateTime>2023-05-26T16:05:55-07:00</MessageCreatedDateTime>
   </MessageHeader>
   <ResourceList>
     <SoundRecording>
       <SoundRecordingType>MusicalWorkSoundRecording</SoundRecordingType>
       <SoundRecordingId>
         <ISRC>CAZZZ2300001</ISRC>
       </SoundRecordingId>
       <ResourceReference>A1</ResourceReference>
       <ReferenceTitle>
         <TitleText>qa track</TitleText>
       </ReferenceTitle>
       <LanguageOfPerformance>en</LanguageOfPerformance>
       <Duration>PT0M25S</Duration>
       <SoundRecordingDetailsByTerritory>
         <TerritoryCode>Worldwide</TerritoryCode>
         <Title TitleType="FormalTitle">
           <TitleText>qa track</TitleText>
         </Title>
         <Title TitleType="DisplayTitle">
           <TitleText>qa track</TitleText>
         </Title>
         <DisplayArtistName>QA Dev</DisplayArtistName>
         <ResourceReleaseDate>2023</ResourceReleaseDate>
         <PLine>
           <Year>2023</Year>
           <PLineCompany>Monster</PLineCompany>
           <PLineText>Test Label 2019</PLineText>
         </PLine>
         <Genre>
           <GenreText>Dance</GenreText>
           <SubGenre>Electronic</SubGenre>
         </Genre>
         <Genre>
           <GenreText>Electronic</GenreText>
         </Genre>
         <TechnicalSoundRecordingDetails>
           <TechnicalResourceDetailsReference>T1</TechnicalResourceDetailsReference>
           <AudioCodecType>PCM</AudioCodecType>
           <BitRate UnitOfMeasure="kbps">0</BitRate>
           <NumberOfChannels>2</NumberOfChannels>
           <SamplingRate>44100</SamplingRate>
           <BitsPerSample>1411200</BitsPerSample>
           <PreviewDetails>
             <StartPoint>30</StartPoint>
             <Duration>PT30S</Duration>
             <ExpressionType>Informative</ExpressionType>
           </PreviewDetails>
           <File>
             <FileName>A10443ZR0WW1FSAKBM_T1.wav</FileName>
             <FilePath>resources/</FilePath>
             <HashSum>
               <HashSum>Hqx2EKbSLUUqfV/ZrxbyUlq/PPU=</HashSum>
               <HashSumAlgorithmType>SHA1</HashSumAlgorithmType>
             </HashSum>
           </File>
         </TechnicalSoundRecordingDetails>
       </SoundRecordingDetailsByTerritory>
     </SoundRecording>
     <Image>
       <ImageType>FrontCoverImage</ImageType>
       <ImageId>
         <ProprietaryId Namespace="DPID:PADPIDA20145670097">PId234234234234</ProprietaryId>
       </ImageId>
       <ResourceReference>A2</ResourceReference>
       <ImageDetailsByTerritory>
         <TerritoryCode>Worldwide</TerritoryCode>
         <ParentalWarningType>NotExplicit</ParentalWarningType>
         <TechnicalImageDetails>
           <TechnicalResourceDetailsReference>T2</TechnicalResourceDetailsReference>
           <ImageCodecType>JPEG</ImageCodecType>
           <ImageHeight>1500</ImageHeight>
           <ImageWidth>1530</ImageWidth>
           <File>
             <FileName>A10443ZR0WW1FSAKBM_T2.png</FileName>
             <FilePath>resources/</FilePath>
             <HashSum>
               <HashSum>QXgcYgIPes3ItdvDXlpHp+aco/g=</HashSum>
               <HashSumAlgorithmType>SHA1</HashSumAlgorithmType>
             </HashSum>
           </File>
         </TechnicalImageDetails>
       </ImageDetailsByTerritory>
     </Image>
     <Text>
       <TextType UserDefinedValue="MEAD" Namespace="DPID:PADPIDA20145670097">UserDefined</TextType>
       <ResourceReference>A1</ResourceReference>
       <TextDetailsByTerritory>
         <TerritoryCode>Worldwide</TerritoryCode>
       </TextDetailsByTerritory>
     </Text>
   </ResourceList>
   <ReleaseList>
     <Release IsMainRelease="true">
       <ReleaseId>
         <GRid>A10443ZR0WW1FSAKBM</GRid>
         <ICPN>243352321142</ICPN>
         <CatalogNumber Namespace="DPID:PADPIDA20145670097">234234234234</CatalogNumber>
       </ReleaseId>
       <ReleaseReference>R0</ReleaseReference>
       <ReferenceTitle>
         <TitleText>qa release</TitleText>
       </ReferenceTitle>
       <ReleaseResourceReferenceList>
         <ReleaseResourceReference ReleaseResourceType="PrimaryResource">A1</ReleaseResourceReference>
         <ReleaseResourceReference ReleaseResourceType="SecondaryResource">A2</ReleaseResourceReference>
       </ReleaseResourceReferenceList>
       <ReleaseType>Album</ReleaseType>
       <ReleaseDetailsByTerritory>
         <TerritoryCode>Worldwide</TerritoryCode>
         <DisplayArtistName>QA Dev</DisplayArtistName>
         <LabelName>Monster</LabelName>
         <Title TitleType="FormalTitle">
           <TitleText>qa release</TitleText>
         </Title>
         <Title TitleType="DisplayTitle">
           <TitleText>qa release</TitleText>
         </Title>
         <ParentalWarningType>NotExplicit</ParentalWarningType>
         <ResourceGroup>
           <ResourceGroup>
             <Title TitleType="GroupingTitle">
               <TitleText>Component 1</TitleText>
             </Title>
             <SequenceNumber>1</SequenceNumber>
             <ResourceGroupContentItem>
               <SequenceNumber>1</SequenceNumber>
               <ResourceType>SoundRecording</ResourceType>
               <ReleaseResourceReference ReleaseResourceType="PrimaryResource">A1</ReleaseResourceReference>
             </ResourceGroupContentItem>
           </ResourceGroup>
           <ResourceGroupContentItem>
             <ResourceType>Image</ResourceType>
             <ReleaseResourceReference ReleaseResourceType="SecondaryResource">A2</ReleaseResourceReference>
           </ResourceGroupContentItem>
         </ResourceGroup>
         <Genre>
           <GenreText>Dance</GenreText>
           <SubGenre>Electronic</SubGenre>
         </Genre>
         <ReleaseDate IsApproximate="true">2023-05-26</ReleaseDate>
         <OriginalReleaseDate IsApproximate="true">2023-05-26</OriginalReleaseDate>
       </ReleaseDetailsByTerritory>
       <PLine>
         <Year>2023</Year>
         <PLineCompany>Monster</PLineCompany>
         <PLineText>Test Label 2019</PLineText>
       </PLine>
       <CLine>
         <Year>2023</Year>
         <CLineCompany>Monster</CLineCompany>
         <CLineText>© 2023 Monster</CLineText>
       </CLine>
     </Release>
     <Release>
       <ReleaseId>
         <ISRC>CAZZZ2300001</ISRC>
         <ProprietaryId Namespace="DPID:PADPIDA20145670097">243352321142_CAZZZ2300001</ProprietaryId>
       </ReleaseId>
       <ReleaseReference>R1</ReleaseReference>
       <ReferenceTitle>
         <TitleText>qa track</TitleText>
       </ReferenceTitle>
       <ReleaseResourceReferenceList>
         <ReleaseResourceReference ReleaseResourceType="PrimaryResource">A1</ReleaseResourceReference>
       </ReleaseResourceReferenceList>
       <ReleaseType>TrackRelease</ReleaseType>
       <ReleaseDetailsByTerritory>
         <TerritoryCode>Worldwide</TerritoryCode>
         <DisplayArtistName>QA Dev</DisplayArtistName>
         <LabelName>Monster</LabelName>
         <Title TitleType="FormalTitle">
           <TitleText>qa track</TitleText>
         </Title>
         <Title TitleType="DisplayTitle">
           <TitleText>qa track</TitleText>
         </Title>
         <ParentalWarningType>NotExplicit</ParentalWarningType>
         <ResourceGroup>
           <ResourceGroupContentItem>
             <SequenceNumber>1</SequenceNumber>
             <ResourceType>SoundRecording</ResourceType>
             <ReleaseResourceReference ReleaseResourceType="PrimaryResource">A1</ReleaseResourceReference>
           </ResourceGroupContentItem>
         </ResourceGroup>
         <Genre>
           <GenreText>Dance</GenreText>
           <SubGenre>Electronic</SubGenre>
         </Genre>
         <ReleaseDate IsApproximate="true">2023-05-26</ReleaseDate>
         <OriginalReleaseDate IsApproximate="true">2023-05-26</OriginalReleaseDate>
       </ReleaseDetailsByTerritory>
       <PLine>
         <Year>2023</Year>
         <PLineCompany>Monster</PLineCompany>
         <PLineText>Test Label 2019</PLineText>
       </PLine>
       <CLine>
         <Year>2023</Year>
         <CLineCompany>Monster</CLineCompany>
         <CLineText>© 2023 Monster</CLineText>
       </CLine>
     </Release>
   </ReleaseList>
   <DealList>
     <ReleaseDeal>
       <DealReleaseReference>R0</DealReleaseReference>
       <Deal>
         <DealTerms>
           <CommercialModelType>AsPerContract</CommercialModelType>
           <Usage>
             <UseType>AsPerContract</UseType>
           </Usage>
           <TerritoryCode>Worldwide</TerritoryCode>
           <ValidityPeriod>
             <StartDate>2023-05-26</StartDate>
           </ValidityPeriod>
         </DealTerms>
       </Deal>
     </ReleaseDeal>
     <ReleaseDeal>
       <DealReleaseReference>R1</DealReleaseReference>
       <Deal>
         <DealTerms>
           <CommercialModelType>AsPerContract</CommercialModelType>
           <Usage>
             <UseType>AsPerContract</UseType>
           </Usage>
           <TerritoryCode>Worldwide</TerritoryCode>
           <ValidityPeriod>
             <StartDate>2023-05-26</StartDate>
           </ValidityPeriod>
         </DealTerms>
       </Deal>
     </ReleaseDeal>
   </DealList>
 </ern:NewReleaseMessage>
 ```