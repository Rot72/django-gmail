# Setting up Email Functionality in Django (Gmail) with python-decouple

Follow these steps to set up email functionality in your Django project using python-decouple:

1. **Install python-decouple**:
```shell
pip install python-decouple
```

2. **Create a .env file** in the same directory as your manage.py file. Inside this .env file, add your email password like so:
```shell
EMAIL_HOST_PASSWORD=your_email_password
```

3. In your settings.py file, **import the config function from python-decouple** and use it to set EMAIL_HOST_PASSWORD:
```python
from decouple import config

EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_PORT = 587
EMAIL_USE_TLS = True
EMAIL_HOST_USER = 'your_email@gmail.com'  
EMAIL_HOST_PASSWORD = config('EMAIL_HOST_PASSWORD')
```

4. **Define your contact and thank_you views**:
```python
def contact(request):
    if request.method == 'POST':
        name = request.POST.get('name', '')
        email_from = request.POST.get('email', '')
        phone = request.POST.get('phone', '')
        message = request.POST.get('message', '')

        subject = f"New contact form submission from {name}"
        message_comp = f"From: {name}\nEmail: {email_from}\nPhone: {phone}\n\n{message}"
        
        send_mail(
            subject,
            message_comp,
            email_from,
            [settings.EMAIL_HOST_USER],
            fail_silently=False
        )
        return HttpResponseRedirect('thank_you')

    return render(request, 'contact.html')

def thank_you(request):    
    return render(request, 'thank_you.html')
```

5. Finally, **add .env to your .gitignore file** to prevent it from being tracked by Git and pushed to GitHub.
```
.env
```
