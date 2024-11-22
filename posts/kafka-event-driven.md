---
title: "كيفاش نخدمو بـ Kafka و Node.js باش نبنيو Event-Driven Architecture"
subtitle: "Microservices ببساطة هي طريقة باش نبنيو التطبيقات، ماشي كتكون وحدة كبيرة (Monolithic System)، ولكن كيتقسمو على بزاف ديال الخدمات الصغيرة اللي كيتعاونو مع بعضهم."
date: "2024-11-22"
---

<!-- # كيفاش نخدمو بـ Kafka و Node.js باش نبنيو Event-Driven Architecture -->
فهاد l'article غادي نحاولوا نبنيو واحد Event-Driven system بسيط ب Node.JS و ،Kafka. ولكن قبل نشرحو خاص نعرفو شنو هي Event-Driven Architecture و شنو هي Kafka و شنو هوما Microservices.

   > قبل مانبدا فهاد شرح نبغي نكوليك أنه من المستحب تكون عندك شي باركة ديال Docker و NodeJs ولكن مشي إلى ماكانوش هانية


## شنو هوما Microservices؟

### تعريف:
Microservices ببساطة هي طريقة باش نبنيو التطبيقات، ماشي كتكون وحدة كبيرة (Monolithic System)، ولكن كيتقسمو على بزاف ديال الخدمات الصغيرة اللي كيتعاونو مع بعضهم.

![images/linked_list](/images/kafka/microserviceArchitectureSimplified.png)
### مثال:
تخيل واحد الريسطو كبير. عوض ما يكون **chef** واحد كيدير كلشي، غادي يكون كاين **chef** ديال البيتزا، **chef** ديال الباستا، و**chef** ديال الديسير. كل واحد مركز غير فخدمتو، وهذا كيسهل التنظيم وتحسين الجودة ديال كل طبق.


### كيفاش كتخدم؟
- **Microservices** كيقسمو التطبيق إلى **Services** صغيرة.
- كل **Service** كتتكلف بحاجة وحدة، بحال الحسابات ديال المستخدمين، الأداء، ولا **Notifications**.
- هاد **Services** كيتواصلو مع بعضياتهم غالباً عبر الإنترنت.

### مميزات:
- **سهولة التحديث والتطوير**: تقدر تحدث أو تصلح تطبيقك بلا ما تأثر على باقي الأجزاء.
- **توسيع النظام (Scaling)**: تقدر تكبر جزء من التطبيق بلا ما تزيد الحمل على باقي الأجزاء.

---

## طرق التواصل بين Microservices
![images/linked_list](/images/kafka/Microservices+Commonication+Frameworks_Synchonous_Asynchronous_EQengineered.png)

### 1. **التواصل المتزامن (Synchronous Communication):**
الخدمات كيتواصلو مع بعضياتهم باستخدام بروتوكولات بحال **HTTP/REST** ولا **gRPC**.

**مثال**:
سيرفيس ديال **Orders** كتصيفط طلب لسيرفيس ديال **Payment** وككتسنى **Response** (بحال تأكيد عملية الدفع). 

### 2. **التواصل غير المتزامن (Asynchronous Communication):**
الخدمات كيرسلو رسائل بلا ما يبقاو يستناو الرد، وكيستعملو **Message Brokers** بحال **Kafka**، **RabbitMQ**، ولا **Amazon SQS**.

**مثال**:
سيرفيس ديال **Orders** كتنشر رسالة فواحد **Queue** كاتقول: "كاين طلب جديد". السيرفيس ديال **Inventory** و**Shipping** كيسمعو لهاد الرسالة وكيبداو خدمتهوم.

---

## شنو هي Kafka؟

### تعريف:
نقدروا نشبهوا **Kafka** بالبوسطا: كتوفر ل **Services** طريقة باش يصيفطوا ويتلقاو البيانات بيناتهم. بلا ما يصيفطو مباشرة، كيتسخدمو **Kafka** كوسيط (**Broker**).

![images/linked_list](/images/kafka/apache-kafka-architecture.webp)
### كيفاش خدامة Kafka؟
- **Producers**: هما لي كيرسلو الرسائل.
- **Consumers**: هما لي كيقراو الرسائل.
- **Topics**: بحال **Mailboxes** فالبوسطا، فين كيتصنفو الرسائل.

### Publish/Subscribe Architecture:
- ال **Producers** كيرسلو الرسائل لواحد **Topic**.
- ال **Consumers** كيقراو الرسائل من داك **Topic**.

### علاش Kafka؟
- **كيتحمل بزاف ديال البيانات**: مزيان للأنظمة اللي خاصها تعالج بزاف ديال الرسائل بسرعة.
- **موثوق**: حتى إذا طاحت شي **Service**، كيبقى الرسائل محفوظة.
- **قابل للتوسيع (Scalable)**: يقدر يخدم مع نظام كيكبر بسهولة.

### مثال:
تخيل موقع ديال التجارة الإلكترونية:
- ملي شي واحد يدير **Order**، السيرفيس ديال **Orders** كترسل رسالة ل **Topic**: "الطلبات اللي تدارت".
- سيرفيس ديال **Inventory** كيقرا الرسالة وكيحدث الستوك.
- سيرفيس ديال **Shipping** كيقرا نفس الرسالة باش يوجدو للإرسال.

---

## شنو هي Event-Driven Architecture؟

### تعريف:
كيفما شفنا ف **Kafka**، كاين واحد **Actor** كيبدا واحد **Event**، وواحد آخر كيتسنا داك **Event** يوقع. ف **Kafka**، سمايناهم **Producer** و **Consumer**.

![images/linked_list](/images/kafka/0_k9vCsZDxVn27YWV0.jpg)
**Event-Driven Architecture** هي طريقة لبناء الأنظمة بحيث الأحداث (**Events**) كتحرك العمليات.

### شنو هو Event؟
أي حاجة كتوقع فالنظام، بحال:
- مستخدم دار **Login**.
- واحد ال **Item** تمسحات.
- دارت **Payment**.

بحال شي رسالة كتكول ليك: "هاد الحاجة طرات".

### كيفاش كتخدم؟
1. **Producers (Event Emitters):** هنا فين كيبدا ال **Event**.
2. **Broker (Event Stream):** هنا فين كيوصلو ال **Events**.
3. **Consumers (Event Listeners):** هما لي كيتفاعلو مع ال **Event**.

### علاش Event-Driven Architecture؟
- **أنظمة منفصلة (Decoupled):** كل جزء مستقل وخدام بوحدو.
- **قابلة للتوسيع (Scalable):** تقدر تزيد أو تنقص ف **Services** بلا مشاكل.
- **Real-time updates:** التفاعلات كتوقع في نفس اللحظة.

---

## أجي نقادو شي حاجة بهادشي لي هضرنا عليه

باركا علينا من هاد الهضرة و أجي نديرو واحد ال 2 Services لي وحدا غادا تصيفط Message لي غادي يكون هو Body ديال Request و Service لاخر غادي يستقبل ال Message و ديك ساعة نشوفو شنو نديرو بيه.

### شنو غادي نحتاجو؟
> غادي نحتاجو Kafka, Zookeeper, nodejs, kafkajs, express, Docker 

---

#### أجي نبداو ب setup ديال Kafka و Zookeeper.

باش نخدمو Kafka غادي يخصنا Zookeeper هاد ساعة ماغاديش نهضرو على Zookeeper نقدرو نهضرو عليه من بعد ولكن لي خصك تعرف عليه هو المسؤول على synchronizing ديال data بين ال Brokers و ال Partitions
نقدرو ندويو عليه كتر فشي Article آخر.

### 1. أولا نصاوبو Directory فين غادي نخدمو. 


![images/linked_list](/images/kafka/kafka-main-directory.png)


### 2. نصاوبو واحد docker-compose.yml
![images/linked_list](/images/kafka/docker-compose.png)

#### و هادا هو docker-compose file ديالنا
![images/linked_list](/images/kafka/docker-content.png)




هاد docker compose file كيدير ال setup ديال zookeeper و Kafka مغاديش نفصل فيه كانـديرو Pull لل images ديال zookeep و kafka كانعطيو Environment Variables باش تقدر kafka تواصل مع zookeeper.

### Service 1
#### 3. وسط kafka-eda نصاوبو Directory ل service1 


![images/linked_list](/images/kafka/createdirservice1.png)


#### 4. ونبداو واحد node project

![images/linked_list](/images/kafka/npm-init.png)

#### 5. نصاوبو index.js file و نأنصطاليو dependencies ديالنا (express, kafkajs)

![images/linked_list](/images/kafka/touchindex1.png)
دابا أجي نأنصطاليو dependencies ديالنا (express, kafkajs)
![images/linked_list](/images/kafka/dependency.png)

#### 6.  نكتبو الكود
Service1 غادا تكون هي ال producer او service لي غادا تصيفط ال Message. 

![images/linked_list](/images/kafka/service1Start.png)

- نديرو Setup ديال Kafka و عيطو على Kafka
- نحددو شمن PORT بغينا نخدو بيه أنا غادي نخدم ب 4000
- نعيطو على express باش نبداو express app
- نزيدو ال middleware لي غادي يتكلف بال Parsing ديال JSON request bodies

---



![images/linked_list](/images/kafka/kafkaClient.png)
دابا نعيطو على Kafka class لي كتوفرو لينا KafkaJS 

غادي نديرو initialization لهاد ال class ولكن غادي يخصنا نعطيوه clientId لي هو واحد ل identifier ديال ال APP. ومن بعد خصنا نوفرو على الأقل Broker واحد لل Kafkajs.

دابا أجي نصيبو ال Producer ديالنا لي هو لي غادي يصيفط Message ل Broker ديالنا لي غادي تكون Endpoint كاتاخد request body و تحول JSON إلى Strigified JSON.

- أولا نعيطو لل Producer :

![images/linked_list](/images/kafka/producer.png)

- وهادي هي ال endpoint ديالنا:

![images/linked_list](/images/kafka/endpoint.png)

1. نبداو Connection مع ال Producer ديالنا
2. نشدو ال Body ونردوه string لي غادي يكون هو ال Message ديالنا
3. نصيفطو ال Message ل Topic لي سميناه test-topic
4. نرجعو Response كيفما كانت غير باش يكون عندنا واحد ال Feedback


و صافي هادي هو ال Producer نقدرو نتعامو و نديرو لي بغينا فداتا لي جايا من Body. أنا غادي نخليها هاكا و نبدا ال App.

![images/linked_list](/images/kafka/listen4000.png)

و هدا هو الكود ديالنا كامل:


![images/linked_list](/images/kafka/Service1code.png)

دابا أجي ندوزو لل Service2 لي غادي تكون هي ال consumer ديالنا:

### Service 2
#### 1. وسط kafka-eda نصاوبو Directory ل service1 
![images/linked_list](/images/kafka/startService2.png)
#### 2. ونبداو  node project
![images/linked_list](/images/kafka/npm-init.png)
#### 3. نصاوبو index.js file

![images/linked_list](/images/kafka/touchindex1.png)

#### 4. نأنصطاليو dependencies (express, kafkajs)

![images/linked_list](/images/kafka/dependency.png)

#### 5. أجي نكتبو الكود

![images/linked_list](/images/kafka/startService22.png)

فهاد الجزء متبدل والو من غير ال PORT بلاصت 4000 درنا 4001 حيت ال َapps بجوج غادي يكونو خدامين فنس الوقت.

![images/linked_list](/images/kafka/iiffe.png)

فهاد service2 مغاديش يكون عندنا endpoints هادي Immediately Invoked Function غادي تبدا مع البداية ديال ال App.

1. نصيبو consumer و نعطيوه groupId. لاش كيصلاح هاد groupId منين كايكونو عندنا بزاف ديال consumer instances كينتاميو لنفس group كيفرقو consumption load  بيناتهم (كل وحدة وشحال كتاخد) و هاد process كايتسمى "consumer group rebalancing" و group هو ال consumers لي عندهم نفس groupId.
2. نبداو Connection مع ال consumer ديالنا
3. نديرو subscribe ل topic لي بغينا نسمعو لل event و Messages لي غادي يجيو فيه كنعطيوه topic و كانزيدو واحد ال argument لي ماشي ضروري غير إلى بغينا نقراو Messages لي فهاد ال topic منين بدا
4.من بعد كنديرو run لل consumer و كنبداو نتصنتو لل Messages 

و هدا هو الكود كامل:


![images/linked_list](/images/kafka/service2all.png)

هدا ما كان دبا إلى مشينا ل Postman وصيفطنا request لل endpoint لي صاوبنا و حلينا terminal ديال service2 أنقدرو نشوفو body ديال request لي كانصيفطو كيجينا كـ Stringified JSON

![images/linked_list](/images/kafka/requestEnd.png)
![images/linked_list](/images/kafka/responseEnd.png)

غادي نخلي ليكم Github Repo ديال هاد Tuto [هنا](https://github.com/soulaimane2/event-driven-dev).

هادا ماكان فهاد tuto و لي عندو شي سؤال يخليه فال comments شكرا على وقتكم.


###### Soulaimane Négra


