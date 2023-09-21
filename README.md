# Razorpay 
### Prerequisite
<hr>

* [Python](https://www.python.org/downloads/)
* [Django](https://docs.djangoproject.com/en/4.0/topics/install/)
* [DRF](https://www.django-rest-framework.org/)
* [Razorpay Account](https://razorpay.com/)


<br>

### Virtual Environment
```python
python3 -m virtualenv myvenv
source myenv/bin/activate
```

### Install requirements
```python
pip install -r requirements.txt
```
<br>

### Setup
<hr>



#### Database Migrations
```python
python Manage.py makemigrations
python manage.py migrate
```

### Start Project
```python
python manage.py runserver
```
<br>

### Request, Response

<br>
The request body should something like this 

```json
{
    "amount": 200,
    "currency": "INR",
    "description": "buying abc",
    "customer": {
        "email": "shahhetu.hs@gmail.com",
        "contact": "8866911353"
    },
    "notify": {
        "email": true,
        "sms": true
    },
    "accept_partial": false
}
```
<br>

Which would return response like : 
```json
{
    "accept_partial": false,
    "amount": 20000,
    "amount_paid": 0,
    "callback_method": "get",
    "callback_url": "http://127.0.0.1:8000/create_payment_link/callback-url/",
    "cancelled_at": 0,
    "created_at": 1662630384,
    "currency": "INR",
    "customer": {
        "contact": "8866911353",
        "email": "shahhetu.hs@gmail.com"
    },
    "description": "buying abc",
    "expire_by": 0,
    "expired_at": 0,
    "first_min_partial_amount": 0,
    "id": "plink_KFMTVyweoAqsnC",
    "notes": null,
    "notify": {
        "email": true,
        "sms": true
    },
    "payments": null,
    "reference_id": "f487fb06-9b5a-445f-a5af-a63b4316e0ad",
    "reminder_enable": false,
    "reminders": [],
    "short_url": "https://rzp.io/i/itOmoOuBB",
    "status": "created",
    "updated_at": 1662630384,
    "upi_link": false,
    "user_id": ""
}
```