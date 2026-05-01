# Comparison of Python pipeline   destinationBankAccountId=13568 amount=2500000000
```

> در صورت فراخوانی درست، پاسخ به این صورت خواهد بود:

```json
{
  "status": "ok",
  "result": {
    "id": "CW430542",
    "createdAt": "2021-12-11T10:13:42.957103+00:00",
    "status": "New",
    "amount": "2500000000",
    "fee": "500000",
    "fulfilledAmount": "499500000",
    "bankAccountId": 13568,
    "bankAccountInfo": "صادرات: IR670190123456789001234567",
    "isCancelable": true,
    "records": [
      {
        "amount": "1000000000",
        "bankReferenceNumber": null,
        "status": "Pending",
        "estimatedSettleAt": null,
        "providerUpdatedAt": "2021-12-11T10:13:42.957103+00:00",
        "transferType": "normal"
      },
      {
        "amount": "1000000000",
        "bankReferenceNumber": "35413545",
        "status": "Failed",
        "estimatedSettleAt": "2021-12-11T10:13:42.957103+00:00",
        "providerUpdatedAt": "2021-12-11T10:13:42.957103+00:00",
        "transferType": "paya"
      },
      {
        "amount": "499500000",
        "bankReferenceNumber": "35415784546",
        "status": "Transferred",
        "estimatedSettleAt": "2021-12-11T10:13:42.957103+00:00",
        "providerUpdatedAt": "2021-12-11T10:13:42.957103+00:00",
        "transferType": "satna"
      }
    ]
  }
}
```


> در صورتی که درخواست با خطا مواجه شود، پاسخ به این شکل خواهد بود

```json
{
  "status": "failed",
  "code": "WithdrawUnavailable",
  "message": "WithdrawUnavailable"
}
```


برای ثبت درخواست برداشت ریالی از این نوع درخواست استفاده نمایید:

* **درخواست:** `POST /cobank/withdraw`
* **<a href="/#ratelimit">محدودیت فراخوانی:</a>** 10 درخواست در 3 دقیقه

### پارامترهای ورودی

| پارامتر                 | نوع      | پیش‌فرض  | توضیحات                 | نمونه                                |
|-------------------------|----------|---------|-------------------------|--------------------------------------|
| destinationBankAccountId| int      | الزامی  | شناسه حساب‌بانکی کاربر   | 13568                                |
| amount                  | monetary | الزامی  | مقدار (ریال)            | 2500000000                           |

* **destinationBankAccountId**: [شناسه حساب بانکی](/#88cb25e727 "دریافت از پروفایل") تایید شده کاربر جهت دریافت وجه 


### پارامترهای خروجی

| پارامتر                 | نوع      | توضیحات                 | نمونه                               |
|-------------------------|----------|-------------------------|--------------------------------------|
| id                      | int      |شناسه درخواست برداشت ریالی که یک رشته خواهد بود و با WJ یا CW آغاز میشود و به دنبال آن عدد خواهد آمد.   | CW13568                            |
| createdAt               | string   | زمان ایجاد درخواست برداشت                                                                              | 2021-12-11T10:13:42.957103+00:00   |
| status                  | string   | وضعیت درخواست برداشت                                                                                   | New                                |
| amount                  | monetary | مقدار درخواست برداشت (ریال)                                                                            | 2500000000                         |
| fee                     | monetary | مقدار کارمزد درخواست برداشت (ریال)                                                                     | 20000                              |
| fulfilledAmount         | monetary | مقدار برداشت انجام شده. (در ابتدا صفر است)                                                             | 1500000000                         |
| bankAccountId           | int      | شناسه حساب‌بانکی کاربر                                                                                  | 13568                              |
| bankAccountInfo         | string   | اطلاعات حساب‌بانکی شامل نام بانک و شماره حساب                                                            | صادرات: IR670190123456789001234567 |
| isCancelable            | bool     | امکان لغو درخواست وجود دارد یا خیر.                                                                    | true, false                        |
| records                 | array    | لیست ریزتراکنش‌ها در صورت وجود                                                                          |                                    |

### پارامترهای فیلد records

| پارامتر                 | نوع       | توضیحات                                         | نمونه                             |
|-------------------------|-----------|-------------------------------------------------|-----------------------------------|
| amount                  | monetary  | مقدار درخواست برداشت ارسال شده به بانک (ریال)   | 1000000000                        |
| bankReferenceNumber     | string    | کد پیگیری بانک                                  | 123f345345g34634                  |
| status                  | string    | وضعیت ریز تراکنش‌                                | Pending, Failed, Transferred      |
| estimatedSettleAt       | string    | زمان تخمینی واریز وجه به حساب کاربر             | 2021-12-11T10:13:42.957103+00:00  |
| providerUpdatedAt       | string    | زمان بروزرسانی وضعیت ریزتراکنش از سمت پروایدر   | 2021-12-11T10:13:42.957103+00:00  |
| transferType            | string    | نوع انتقال وجه                                  | normal, paya, satna               |


###  وضعیت‌های درخواست برداشت

| وضعیت                | توضیحات                                         |
|----------------------|-------------------------------------------------|
| New                  | ثبت درخواست توسط کاربر                          | 
| Sent                 | ارسال درخواست به پروایدر                        | 
| Bank processing      | پردازش درخواست توسط بانک                        | 
| Partially Done       | بخشی از درخواست موفق                            |
| Done                 | درخواست کاملا موفق                               | 
| Failed               | درخواست کاملا ناموفق                             | 
| Rejected             | رد درخواست (توسط نوبیتکس، پروایدر یا بانک)      | 
| Canceled             | لغو درخواست توسط کاربر                          | 


### حالت‌های خطا

در صورتی که درخواست برداشت معتبر نباشد، ممکن است یکی از این خطاها برگردانده شود. در صورت دریافت هر یک از این خطاها، درخواست شما ثبت نشده است و در صورت تمایل باید درخواست را دوباره ارسال کنید.

کد خطا  |                                                                                             توضیحات
----------------------------------------------------------------------------------------------------| ---------
UnAcceptedDisclaimerError |                                            برای برداشت، لازم است موجودی کیف اسپات خود را تایید کنید.
FeatureUnavailable |                                                                    این امکان فعلا در دسترس شما نیست.
ParseError |                                                                   مشکلی در پارامتر ورودی وجود دارد.
BankAccountNotFound |                                                                       حساب بانکی مورد نظر پیدا نشد.
TooManyRequests |                                                             قبلا درخواست داده‌اید، لطفا کمی صبر کنید.
WithdrawUnavailable |                                                                برداشت ریالی برای شما محدود شده است.
WithdrawAmountLimitation |                                          مقدار برداشت نباید از حداکثر مقدار قابل برداشت بیشتر باشد.
InsufficientBalance |                                                                                   موجودی کافی نیست.
AmountTooLow |                                            مقدار برداشت نباید از حداقل مقدار قابل برداشت کمتر باشد.
InsufficientBalanceOrInactiveWallet |                                                                                   موجودی کافی نیست.
WithdrawLimitReached |                                 در هر ۲۴ ساعت فقط ۳ برداشت ریالی و ۱۰ برداشت رمزارزی امکان‌پذیر است.
AmountTooHigh |                                          مقدار برداشت نباید از حداکثر مقدار قابل برداشت بیشتر باشد.
ShabaWithdrawCannotProceed |   سقف واریز به هر شماره شبا ۲ میلیارد ریال است. می‌توانید مبلغ را به دو یا چند شماره شبا واریز کنید.



### نکات و ملاحظات
مقدار estimatedSettleAt و bankReferenceNumber در ابتدای ثبت درخواست خالی می‌باشد و پس از دریافت اطلاعات از سمت بانک، این فیلدها مقداردهی خواهد شد.


<h2 id="rial-withdraw-cancel">لغو درخواست برداشت ریالی</h2>

```shell
curl -X POST 'https://apiv2.nobitex.ir/cobank/withdraw/<id>/cancel' \
  -H 'Authorization: Token yourTOKENhereHEX0000000000' \
  -H 'Content-Type: application/json'
```

```plaintext
http POST https://apiv2.nobitex.ir/cobank/withdraw/<id>/cancel 
```

> در صورت فراخوانی درست، پاسخ به این صورت خواهد بود:

```json
{
  "status": "ok",
  "result": {
    "id": "CW430542",
    "createdAt": "2021-12-11T10:13:42.957103+00:00",
    "status": "Canceled",
    "amount": "2500000000",
    "fee": "500000",
    "fulfilledAmount": "499500000",
    "bankAccountId": 13568,
    "bankAccountInfo": "صادرات: IR670190123456789001234567",
    "isCancelable": false,
    "records": [
      {
        "amount": "1000000000",
        "bankReferenceNumber": null,
        "status": "Pending",
        "estimatedSettleAt": null,
        "providerUpdatedAt": "2021-12-11T10:13:42.957103+00:00",
        "transferType": "normal"
      },
      {
        "amount": "1000000000",
        "bankReferenceNumber": "35413545",
        "status": "Failed",
        "estimatedSettleAt": "2021-12-11T10:13:42.957103+00:00",
        "providerUpdatedAt": "2021-12-11T10:13:42.957103+00:00",
        "transferType": "paya"
      },
      {
        "amount": "499500000",
        "bankReferenceNumber": "35415784546",
        "status": "Transferred",
        "estimatedSettleAt": "2021-12-11T10:13:42.957103+00:00",
        "providerUpdatedAt": "2021-12-11T10:13:42.957103+00:00",
        "transferType": "satna"
      }
    ]
  }
}
```


> در صورت مواجه با خطا چنین پاسخی خواهید داشت


```json
{
  "status": "failed",
  "code": "WithdrawRequestNotFound",
  "message": "Withdraw Request Not Found"
}
```

برای لغو درخواست برداشت ریالی از این نوع درخواست استفاده نمایید:

* **درخواست:** `POST /cobank/withdraw/<id>/cancel`
* **<a href="/#ratelimit">محدودیت فراخوانی:</a>** 60 درخواست در 1 ساعت
* **<a href="/#ratelimit">محدودیت فراخوانی:</a>** 10 درخواست در 1 دقیقه

### پارامترهای ورودی

| پارامتر  | نوع    | پیش‌فرض                    | توضیحات                 | نمونه       |
|----------|--------|-------------------------- |-------------------------|-------------|
| id       | string | الزامی                    | شناسه درخواست برداشت    | CW256854    |


### نکات و ملاحظات
امکان لغو درخواست برداشت تا زمانی ممکن است که وضعیت درخواست در حالت New باشد و بیش از ۳ دقیقه از زمان ثبت آن نگذشته باشد.


### حالت‌های خطا

کد خطا  |                                                   توضیحات
--------------------------------------------------------- | ---------
WithdrawRequestNotFound |                                  درخواست برداشت پیدا نشد.
UnAcceptedDisclaimerError |  برای برداشت، لازم است موجودی کیف اسپات خود را تایید کنید.
CancellationFailed |                             لغو درخواست برداشت ناموفق شد.
NotCancellable |                        لغو درخواست برداشت امکان‌پذیر نیست.







<h2 id="rial-withdraw-details">مشاهده جزئیات برداشت ریالی</h2>

```shell
curl -X GET 'https://apiv2.nobitex.ir/cobank/withdraw/<id>' \
  -H 'Authorization: Token yourTOKENhereHEX0000000000' \
  -H 'Content-Type: application/json'
```

```plaintext
http GET https://apiv2.nobitex.ir/cobank/withdraw/<id> 
```

> در صورت فراخوانی درست، پاسخ به این صورت خواهد بود:

```json
{
  "status": "ok",
  "result": {
    "id": "CW430542",
    "createdAt": "2021-12-11T10:13:42.957103+00:00",
    "status": "New",
    "amount": "2500000000",
    "fee": "500000",
    "fulfilledAmount": "499500000",
    "bankAccountId": 13568,
    "bankAccountInfo": "صادرات: IR670190123456789001234567",
    "isCancelable": true,
    "records": [
      {
        "amount": "1000000000",
        "bankReferenceNumber": null,
        "status": "Pending",
        "estimatedSettleAt": null,
        "providerUpdatedAt": "2021-12-11T10:13:42.957103+00:00",
        "transferType": "normal"
      },
      {
        "amount": "1000000000",
        "bankReferenceNumber": "35413545",
        "status": "Failed",
        "estimatedSettleAt": "2021-12-11T10:13:42.957103+00:00",
        "providerUpdatedAt": "2021-12-11T10:13:42.957103+00:00",
        "transferType": "paya"
      },
      {
        "amount": "499500000",
        "bankReferenceNumber": "35415784546",
        "status": "Transferred",
        "estimatedSettleAt": "2021-12-11T10:13:42.957103+00:00",
        "providerUpdatedAt": "2021-12-11T10:13:42.957103+00:00",
        "transferType": "satna"
      }
    ]
  }
}
```


> در صورت مواجه با خطا چنین پاسخی خواهید داشت


```json
{
  "status": "failed",
  "code": "WithdrawRequestNotFound",
  "message": "Withdraw Request Not Found"
}
```

برای مشاهده جزئیات درخواست برداشت ریالی از این نوع درخواست استفاده نمایید:

* **درخواست:** `POST /cobank/withdraw/<id>`
* **<a href="/#ratelimit">محدودیت فراخوانی:</a>** 60 درخواست در 2 دقیقه

### پارامترهای ورودی

| پارامتر  | نوع    | پیش‌فرض                    | توضیحات                 | نمونه       |
|----------|--------|-------------------------- |-------------------------|-------------|
| id       | string | الزامی                    | شناسه درخواست برداشت    | CW256854    |



### حالت‌های خطا

کد خطا  |                                                   توضیحات
--------------------------------------------------------- | ---------
WithdrawRequestNotFound |                                  درخواست برداشت پیدا نشد.
UnAcceptedDisclaimerError |  برای برداشت، لازم است موجودی کیف اسپات خود را تایید کنید.
|-------------------------------------------------------------------------|---------|-------|--------|----------|-------|-----------------|
| Developer, Maintainer                                             |  Airbnb, Apache | Spotify | M3 | Netflix | Quantum-Black (McKinsey) | Yusuke Minami |
| Wrapped packages                                                        |         |       | Luigi  |          |       | Kedro, MLflow   |
| Easiness/flexibility to define DAG                                      |         |       | 👍      | 👍        | 👍     | 👍👍             |
| Modularity of DAG definition                                            | 👍👍       |       |        |          | 👍👍     | 👍👍               |
| Unstructured data can be passed between tasks                           |         | 👍👍     | 👍👍      | 👍👍        | 👍👍     | 👍👍               |
| Built\-in various data (file/database) existence check wrappers         |         | 👍👍   | 👍👍    |          | 👍👍   | 👍👍             |
| Built\-in various data (file/database) operation (read/write) wrappers  |         |       | 👍      |          | 👍👍   | 👍👍             |
| Modularity, reusability, testability of data operation                  |         |       | 👍      |          | 👍👍   | 👍👍             |
| Automatic resuming option by detecting the intermediate data            |         | 👍👍     | 👍👍      |          |       | 👍👍               |
| Force rerun of tasks by detecting parameter change                      |         |       | 👍👍      |          |       |                 |
| Save parameters for experiments                                         |         |       | 👍👍      |          |       | 👍👍               |
| Parallel execution                                                      | 👍       | 👍     | 👍      | 👍        | 👍     | 👍               |
| Distributed parallel execution with Celery                              | 👍👍       |       |        |          |       |                 |
| Visualization of DAG                                                    | 👍👍       | 👍     | 👍      |          | 👍     | 👍               |
| Execution status monitoring in GUI                                             | 👍👍     | 👍     | 👍      |          |       |                 |
| Scheduling, Triggering in GUI                                           | 👍       |       |        |          |       |                 |
| Notification to Slack                                                   | 👍       |       | 👍      |          |       |                 |


## Airflow 

https://github.com/apache/airflow

Released in 2015 by Airbnb.

Airflow enables you to define your DAG (workflow) of tasks in Python code (an independent Python module).

(Optionally, unofficial plugins such as [dag-factory](https://github.com/ajbosco/dag-factory) enables you to define DAG in YAML.)

### Pros:

- Provides rich GUI with features including DAG visualization, execution progress monitoring, scheduling, and triggering.
- Provides distributed computing option (using Celery).
- DAG definition is modular; independent from processing functions.
- Workflow can be nested using `SubDagOperator`.
- Supports Slack notification.

### Cons:

- Not designed to pass data between dependent tasks without using a database. 
There is no good way to pass unstructured data (e.g. image, video, pickle, etc.) between dependent tasks in Airflow.
- You need to write file access (read/write) code. 
- Does not support automatic pipeline resuming option using the intermediate data files or databases.


## Luigi

https://github.com/spotify/luigi

Released in 2012 by Spotify.

Luigi enables you to define your pipeline by child classes of `Task` with 3 class methods (`requires`, `output`, `run`) in Python code.

### Pros:

- Support automatic pipeline resuming option using the intermediate data files in local or cloud (AWS, GCP, Azure) or databases as defined in `Task.output` method using `Target` class.
- You can write code so any data can be passed between dependent tasks.
- Provides GUI with features including DAG visualization, execution progress monitoring.

### Cons:

- You need to write file/database access (read/write) code.
- Pipeline definition, task processing (Transform of ETL), and data access (Extract&Load of ETL) are tightly coupled and not modular. You need to modify the task classes to reuse in future projects.


## Gokart

https://github.com/m3dev/gokart

Released in Dec 2018 by M3.

Gokart works on top of Luigi. 

### Pros: 

In addition to Luigi's advantages:
- Can split task processing (Transform of ETL) from pipeline definition using `TaskInstanceParameter` so you can easily reuse them 
in future projects.
- Provides built-in file access (read/write) wrappers as `FileProcessor` classes for pickle, npz, gz, txt, csv, tsv, json, xml.
- Saves parameters for each experiment to assure reproducibility. Viewer called [thunderbolt](https://github.com/m3dev/thunderbolt) can be used.
- Reruns tasks upon parameter change based on hash string unique to the parameter set in each intermediate file name. 
This feature is useful for experimentation with various parameter sets.
- Syntactic sugar for Luigi's `requires` class method using class decorator. 
- Supports Slack notification.

### Cons:

- Supported data formats for file access wrappers are limited. You need to write file/database access (read/write) code to use unsupported formats.


## Metaflow

https://github.com/Netflix/metaflow

Released in Dec 2019 by Netflix.

Metaflow enables you to define your pipeline as a child class of `FlowSpec` that includes class methods with `step` decorators in Python code.

### Pros:

- Integration with AWS services (Especially AWS Batch).

### Cons:

- You need to write file/database access (read/write) code.
- Pipeline definition, task processing (Transform of ETL), and data access (Extract&Load of ETL) are tightly coupled and not modular. You need to modify the task classes to reuse in future projects.
- Does not support GUI. 
- Not much support for GCP & Azure.
- Does not support automatic pipeline resuming option using the intermediate data files or databases.


## Kedro

https://github.com/quantumblacklabs/kedro

Released in May 2019 by QuantumBlack, part of McKinsey & Company.

Kedro enables you to define pipelines using list of `node` functions with 3 arguments 
(`func`: task processing function, `inputs`: input data name (list or dict if multiple), `outputs`: output data name (list or dict if multiple)) 
in Python code (an independent Python module).

### Pros:

- Provides built-in file/database access (read/write) wrappers as `DataSet` classes for 
CSV, Pickle, YAML, JSON, Parquet, Excel, and text in local or cloud (S3 in AWS, GCS in GCP), as well as SQL, Spark, etc. 
- Any data format support can be added by users. 
- Pipeline definition, task processing (Transform of ETL), and data access (Extract&Load of ETL) are independent and modular. 
You can easily reuse in future projects.
- Pipelines can be nested. (A pipeline can be used as a sub-pipeline of another pipeline. )
- GUI ([kedro-viz](https://github.com/quantumblacklabs/kedro-viz)) provides DAG visualization feature.

### Cons:
- Does not support automatic pipeline resuming option using the intermediate data files or databases.
- GUI ([kedro-viz](https://github.com/quantumblacklabs/kedro-viz)) does not provide execution progress monitoring feature.
- Package dependencies which are not used in many cases (e.g. pyarrow) are included in the `requirements.txt`.


## PipelineX:

https://github.com/Minyus/pipelinex

Released in Nov 2019 by a Kedro user (me).

PipelineX works on top of Kedro and MLflow.

PipelineX enables you to define your pipeline in YAML (an independent YAML file).

### Pros:

In addition to Kedro's advantages:
- Supports automatic pipeline resuming option using the intermediate data files or databases.
- Optional syntactic sugar for Kedro Pipeline. (e.g. Sequential API similar to PyTorch (`torch.nn.Sequential`) and Keras (`tf.keras.Sequential`))
- Optional syntactic sugar for Kedro `DataSet` catalog. (e.g. Use file name in the file path as the dataset instance name)
- Backward-compatible to pure Kedro.
- Integration with MLflow to save parameters, metrics, and other output artifacts such as models for each experiment.
- Integration with common packages for Data Science: PyTorch, Ignite, pandas, OpenCV.
- Additional `DataSet` including image set (a folder including images) useful for computer vision applications.
- Lean project template compared with pure Kedro.

### Cons:

- GUI ([kedro-viz](https://github.com/quantumblacklabs/kedro-viz)) does not provide execution progress monitoring feature.
- Package dependencies which are not used in many cases (e.g. pyarrow) are included in the `requirements.txt` of Kedro.
- PipelineX is developed and maintained by an individual (me) at this moment.




## Platform-specific options

### Argo

https://github.com/argoproj/argo

Uses Kubernetes to run pipelines.

### Kubeflow Pipelines

https://github.com/kubeflow/pipelines

Works on top of Argo.

### Oozie

https://github.com/apache/oozie

Manages Hadoop jobs.


### Azkaban

https://github.com/azkaban/azkaban

Manages Hadoop jobs.

### GitLab CI/CD

https://docs.gitlab.com/ee/ci/

- Runs pipelines defined in YAML.
- Supports triggering by git push, CRON-style scheduling, and manual clicking.
- Supports Docker containers.


## References

Airflow
- https://github.com/apache/airflow
- https://airflow.apache.org/docs/stable/howto/initialize-database.html
- https://medium.com/datareply/integrating-slack-alerts-in-airflow-c9dcd155105

Luigi
- https://github.com/spotify/luigi
- https://luigi.readthedocs.io/en/stable/api/luigi.contrib.html
- https://www.m3tech.blog/entry/2018/11/12/110000

Gokart
- https://github.com/m3dev/gokart
- https://www.m3tech.blog/entry/2019/09/30/120229
- https://qiita.com/Hase8388/items/8cf0e5c77f00b555748f

Metaflow
- https://github.com/Netflix/metaflow
- https://docs.metaflow.org/metaflow/basics
- https://docs.metaflow.org/metaflow/scaling
- https://medium.com/bigdatarepublic/a-review-of-netflixs-metaflow-65c6956e168d

Kedro
- https://github.com/quantumblacklabs/kedro
- https://kedro.readthedocs.io/en/latest/03_tutorial/04_create_pipelines.html
- https://kedro.readthedocs.io/en/latest/kedro.io.html#data-sets
- https://medium.com/mhiro2/building-pipeline-with-kedro-for-ml-competition-63e1db42d179
    
PipelineX
- https://github.com/Minyus/pipelinex
    
Airflow vs Luigi
- https://towardsdatascience.com/data-pipelines-luigi-airflow-everything-you-need-to-know-18dc741449b7
- https://medium.com/better-programming/airbnbs-airflow-versus-spotify-s-luigi-bd4c7c2c0791
- https://www.quora.com/Which-is-a-better-data-pipeline-scheduling-platform-Airflow-or-Luigi
 
## Inaccuracies

Please kindly let me know if you find anything inaccurate. 

Pull requests for https://github.com/Minyus/Python_Packages_for_Pipeline_Workflow/blob/master/README.md are welcome.
https://github.com/manuchehrsa