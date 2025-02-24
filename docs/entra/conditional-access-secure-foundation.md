---
title: Conditional Access policy templates for Secure foundation
description: CSoG - Customer Self-onboarding Guides
---
# Conditional Access policy templates for Secure foundation

Conditional Access templates จาก <a href="https://learn.microsoft.com/en-us/entra/identity/conditional-access/concept-conditional-access-policy-common?tabs=secure-foundation">Conditional Access policy templates</a> ช่วยให้เราสามารถสร้าง policy ตามคำแนะนำจาก Microsoft ได้โดยง่าย

Templates มีหลายหมวด ในคู่มือนี้จะทำหมวด Secure foundation ซึ่งเป็น Conditional Access พื้นฐานที่ทุกองค์กรควรทำนะครับ

## Prerequisites

### Review enabled authentication methods

Conditional Access สามารถใช้งานร่วมกับ MFA ได้หลายแบบ เราอาจจะตรวจสอบก่อนว่า tenant ของเรา ได้เปิดใช้งาน MFA ครบทุกแบบตามที่เราต้องการแล้วหรือยัง?

![alt text](../assets/casf/image-56.png)

### Create a pilot user group

Conditional Access ที่ apply เข้าหา All users อาจกระทบการใช้งานโดยไม่คาดคิด ในช่วงเริ่มต้น เราควร apply policy บางตัวเข้าหา pilot user group เพื่อทดสอบก่อน

![alt text](../assets/casf/image-101.png)

## 1. SF01 - Require multifactor authentication for admins

Account ที่มีสิทธิ์บริหารจัดการระบบมักเป็นเป้าหมายของผู้บุกรุก เขาควรถููกบังคับใช้ MFA เพื่อลดความเสี่ยงจากการถูกโจมตี

### 1.1 Create the policy

![alt text](../assets/casf/image-1.png)

![alt text](../assets/casf/image-2.png)

ปรับชื่อ policy ตามความเหมาะสม, รีวิวค่า settings เริ่มต้น, แล้ว Create

![alt text](../assets/casf/image-5.png)

### 1.2 Adjust & turn on the policy

1\. กดเข้าไปใน policy

![alt text](../assets/casf/image-4.png)

2\. ที่ Users > Exclude

- ควรเพิ่ม emergency access หรือ break-glass account จาก <a href="https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/security-emergency-access">Manage emergency access accounts in Microsoft Entra ID</a> 
- และเพิ่ม servie account ที่จำเป็น เช่น service account ที่ไม่รองรับ MFA แต่เป็นสมาชิกของ administrative role ใด role นึงอยู่

![alt text](../assets/casf/image-52.png)

3\. เปิดใช้งาน policy แล้ว Save

![alt text](../assets/casf/image-9.png)

ตอนนี้เราได้ Conditional Access policy สำหรับ Secure foundtaion ตัวแรกแล้วนะครับ

![alt text](../assets/casf/image-10.png)

## 2. SF02 - Securing security info registration

หากเราต้องการให้ผู้ใช้ต้องทำ MFA ก่อนเข้า Security info ของตัวเอง ก็สามารถใช้ policy ตัวนี้ได้

![alt text](../assets/casf/image-24.png)

### 1.1 Create the policy

![alt text](../assets/casf/image-11.png)

ปรับชื่อ policy ตามความเหมาะสม, รีวิวค่า settings เริ่มต้น, แล้ว Create

![alt text](../assets/casf/image-7.png)

### 1.2 Adjust & turn on the policy

1\. กดเข้าไปใน policy

![alt text](../assets/casf/image-13.png)

2\. ที่ Users > Exclude

เพิ่ม break-glass account เข้าไป

![alt text](../assets/casf/image-53.png)

3\. เปิดใช้งาน policy แล้ว Save

![alt text](../assets/casf/image-22.png)

![alt text](../assets/casf/image-23.png)

### 1.3 Test

1\. ทดลองเข้า security info เราจะถูก request MFA

![alt text](../assets/casf/image-24.png)

![alt text](../assets/casf/image-25.png)

2\. แต่ถ้าเราไม่ต้องการให้มี MFA ถ้าผู้ใช้กำลังใช้งานอยู่ใน network ภายในของบริษัท

เราอาจสร้าง location เช่น HQ

- ระบุ public IP ที่ client ใน HQ ใช้ access Internet
- แล้ว mark เป็น trusted locations

![alt text](../assets/casf/image-26.png)

3\. เมื่อลองเข้า Security info จาก HQ network อีกครั้ง

![alt text](../assets/casf/image-24.png)

ก็จะไม่มี MFA prompt ครับ

![alt text](../assets/casf/image-27.png)

## 3. SF03 - Block legacy authentication

Microsoft แนะนำให้เรา block authentication protocol เก่าๆ ที่ไม่รองรับ MFA เช่น basic authentication

### 3.1 Identify legacy authentication use

ก่อนอื่น ให้เราตรวจสอบจาก Sign-in log ว่า ในบริษัทเรามี app ตัวไหนที่ใช้ legacy authentication protocol อยู่ไหม?

ถ้าพบ ให้เราลองพิจารณาดูว่าจะทำอย่างไรนะครับ

![alt text](../assets/casf/image-8.png)

ตัวอย่างเช่น ในภาพนี้ เราพบ SMTP Auth จาก service account ชื่อว่า app1 ที่ยังใช้ basic authentication relay mail มาที่ Exchange Online อยู่ (ควรวางแผนเปลี่ยนไปใช้ OAuth แทน)

![alt text](../assets/casf/image-12.png)

### 3.2 Create the policy

![alt text](../assets/casf/image-28.png)

ปรับชื่อ policy ตามความเหมาะสม, รีวิวค่า settings เริ่มต้น, แล้ว Create

![alt text](../assets/casf/image-14.png)

### 3.3 Adjust & turn on the policy

1\. กดเข้าไปใน policy

![alt text](../assets/casf/image-30.png)

2\. ที่ Users > Exclude

เพิ่ม break-glass account เข้าไป

![alt text](../assets/casf/image-54.png)

3\. เปิดใช้งาน policy แล้ว Save

![alt text](../assets/casf/image-36.png)

![alt text](../assets/casf/image-37.png)

### 3.4 Test

ลองส่่ง mail จาก app1 เข้าหา user ในองค์กร ด้วย basic authentication

```
$Username = "app1@cloud.coms.ws"
$Password = "Jxxxxxxxx"
$SecurePassword = ConvertTo-SecureString -String $Password -AsPlainText -Force
$Creds = New-Object System.Management.Automation.PSCredential ($Username, $SecurePassword)

Send-MailMessage -From "app1@cloud.coms.ws" `
                 -To "user1@cloud.coms.ws" `
                 -Subject "Test Email via Basic Authentication" `
                 -Body "This is a test email." `
                 -SmtpServer "smtp.office365.com" `
                 -UseSsl `
                 -Credential $Creds `
                 -Port 587

```

ซึ่งตอนนี้ เราส่งไม่ได้แล้วนะครับ

![alt text](../assets/casf/image-59.png)

### 3.5 Migitation

สมมติว่า ช่วงนี้เรายังไม่พร้อมจะเปลี่ยนไปใช้ OAuth เราจึง exclude app1 service account ไม่ให้ถูก policy นี้ apply หาขั่วคราวก่อน

>หมายเหตุ: SMTP AUTH ยังใช้ basic authentication ได้ถึง กันยายน 2568 จาก <a href="https://learn.microsoft.com/en-us/exchange/clients-and-mobile-in-exchange-online/deprecation-of-basic-authentication-exchange-online#pop-imap-and-smtp-auth">Deprecation of Basic authentication in Exchange Online</a>
>

![alt text](../assets/casf/image-60.png)

ตอนนี้ app1 ใช้ basic authentication ส่ง mail ได้แล้วนะครับ

![alt text](../assets/casf/image-21.png)

![alt text](../assets/casf/image-62.png)

## 4. SF04 - Require multifactor authentication for admins accessing Microsoft admin portals

### 4.1 Create the policy

1\. สร้าง policy จาก template

![alt text](../assets/casf/image-41.png)

ปรับชื่อ policy ตามความเหมาะสม, รีวิวค่า settings เริ่มต้น, แล้ว Create

![alt text](../assets/casf/image-16.png)

### 4.2 Adjust & turn on the policy

1\. กดเข้าไปใน policy

![alt text](../assets/casf/image-43.png)

2\. ที่ Users > Exclude

เพิ่ม break-glass account เข้าไป

![alt text](../assets/casf/image-55.png)

3\. เปิดใช้งาน policy แล้ว Save

![alt text](../assets/casf/image-49.png)

![alt text](../assets/casf/image-50.png)

## 5. SF05 - Require multifactor authentication for all users

### 5.1 Create the policy

![alt text](../assets/casf/image-63.png)

ปรับชื่อ policy ตามความเหมาะสม, รีวิวค่า settings เริ่มต้น, แล้ว Create

![alt text](../assets/casf/image-17.png)

### 5.2 Adjust & turn on the policy

1\. กดเข้าไปใน policy

![alt text](../assets/casf/image-65.png)

2\. เราต้องการ pilot การใช้งานก่อน

ดังนั้น ให้เราเปลี่ยน All users เป็น G CA Pilot Users group

![alt text](../assets/casf/image-103.png)

3\. ที่ Users > Exclude

เพิ่ม break-glass account และ service account ที่จำเป็นเข้าไป

![alt text](../assets/casf/image-109.png)

4\. เปิดใช้งาน policy แล้ว Save

![alt text](../assets/casf/image-71.png)

![alt text](../assets/casf/image-72.png)

## 6. SF06 - Require multifactor authentication for Azure management

สร้าง policy เพื่อบังคับใช้ MFA เมื่อใช้ Azure management tools เหล่านี้

- Azure portal
- Azure PowerShell
- Azure CLI

### 6.1 Create the policy

![alt text](../assets/casf/image-73.png)

ปรับชื่อ policy ตามความเหมาะสม, รีวิวค่า settings เริ่มต้น, แล้ว Create

![alt text](../assets/casf/image-18.png)

### 6.2 Adjust & turn on the policy

1\. กดเข้าไปใน policy

![alt text](../assets/casf/image-75.png)

2\. ที่ Users > Exclude

เพิ่ม break-glass account เข้าไป

![alt text](../assets/casf/image-77.png)

3\. เปิดใช้งาน policy แล้ว Save

![alt text](../assets/casf/image-80.png)

![alt text](../assets/casf/image-81.png)

## 7. SF07 - Require compliant or Microsoft Entra hybrid joined device or multifactor authentication for all users

หากต้องการ bypass MFA ให้กับผู้ใช้ ที่กำลังใช้งานเครื่อง Entra Hybrid Join หรือ compliant devices (ใน Intune) เราสามารถใช้ policy ตัวนี้ได้

ผู้ใช้ที่ไม่ได้ใช้งานอยู่บนเครื่อง Entra Hybrid Join หรือ compliant devices จะต้องทำ MFA ก่อนเข้าใช้บริการ

### 7.1 Create the policy

![alt text](../assets/casf/image-82.png)

ปรับชื่อ policy ตามความเหมาะสม, รีวิวค่า settings เริ่มต้น, แล้ว Create

![alt text](../assets/casf/image-19.png)

### 7.2 Adjust & turn on the policy

1\. กดเข้าไปใน policy

![alt text](../assets/casf/image-84.png)

2\. เราต้องการ pilot การใช้งานก่อน

ดังนั้น ให้เราเปลี่ยน All users เป็น G CA Pilot Users group

![alt text](../assets/casf/image-104.png)

3\. ที่ Users > Exclude

เพิ่ม break-glass account และ service account ที่จำเป็นเข้าไป

![alt text](../assets/casf/image-110.png)

4\. เปิดใช้งาน policy แล้ว Save

![alt text](../assets/casf/image-89.png)

![alt text](../assets/casf/image-90.png)

## 8. SF08 - Require compliant device

### Don't create this policy

หลายๆ องค์กรอาจยังไม่พร้อมที่จะบังคับว่า device ทุกเครื่องจะต้อง compliant (ใน Intune) ก่อนจะถูกอนุญาตให้เข้าใช้บริการ

หากเรา turn on policy ตัวนี้ ผู้ใช้ที่ใช้งานบนเครื่องที่ไม่เป็น compliant devices อาจพบ sign in error คล้ายๆ แบบนี้

![alt text](../assets/casf/image-112.png)

ดังนั้น ถ้าองค์กรเราจะยังพร้อมที่จะทำข้อนี้ ณ ตอนนี้ ให้เราข้าม policy นี้ไปก่อนนะครับ

![alt text](../assets/casf/image-20.png)

แล้วใช้ policy แค่ 7 ตัว สำหรับ Secure foundation ใน phase แรกนี้ก็พอครับ

![alt text](../assets/casf/image-90.png)

## Next steps

ในส่วนของการ pilot ด้วย G CA Pilot Users group  ให้เราค่อยเพิ่มผู้ใช้เข้าไปเรื่อยๆ แล้วเมื่อพร้อมที่จะบังคับใช้ทั้งองค์กร ก็สามารถเปลี่ยน policy ตัวที่ใช้ G CA Pilot Users group อยู่ ไปใช้ All users ได้เลยนะครับ

## References

- <a href="https://learn.microsoft.com/en-us/entra/identity/conditional-access/concept-conditional-access-policy-common?tabs=secure-foundation">Conditional Access policy templates</a>
- <a href="https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/security-emergency-access">Manage emergency access accounts in Microsoft Entra ID</a>
- <a href="https://learn.microsoft.com/en-us/exchange/clients-and-mobile-in-exchange-online/deprecation-of-basic-authentication-exchange-online#pop-imap-and-smtp-auth">Deprecation of Basic authentication in Exchange Online</a>
>

...