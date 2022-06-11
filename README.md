# HIRE PAGE AND MPESA INTERGRATION
This contains explanation on the hire page and mpesa intergration
## STEP ONE
Create A Table in MYSQL database called 'cars' and it should have the following columns
1. reg
2. make
3. color
4. image
5. description
6. amount

Insert Atleast 15 records and the images you will access it from coding.co.ke/images whereby you will pase the url like this for example:
https://coding.co.ke/images/car1.jpg into the image column 

## STEP TWO
Create a route for retriving the cars and its details from the database and call it /hire


```

@app.route('/hire')
def hire():
    if 'key' in session:
        
        conn = pymysql.connect(host='localhost',user='root',password='',database='sierra')
        cursor = conn.cursor()

        sql = 'select * from cars'
        cursor.execute(sql)
        if cursor.rowcount == 0:
            return render_template('hire.html',msg='No Cars Available')
        else:
            rows=cursor.fetchall()
            return render_template('hire.html',rows=rows)
    else:
        return redirect('/login')


```
Whereby you select all the records from the table
Check if there is any record if there is not you render the template and return a mesage no records
else you create a variable row and fetch the rows and store it and return the template with the rows

## STEP 3
Create a template called hire and bind the rows ou have received in a card
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3" crossorigin="anonymous">
    
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-ka7Sk0Gln4gmtz2MlQnikT1wXgYsOg+OMhuP+IlRH9sENBO0LRn5q+8nbTov4+1p" crossorigin="anonymous"></script>
  
<style>
    .must{
        padding-top: 150px;
  padding-bottom: 150px;
  
  text-align: center;
  color: #fff;
  background-image: url("images/bg-showcase-1.jpg");
  background-repeat: no-repeat;

  background-size: cover;
    }



</style>


</head>
<body>
    <section class="row">
        <div class="col-md-12">
    
            <nav class="navbar navbar-expand-lg navbar-light bg-info">
                <div class="container-fluid">
                <a href="#" class="navbar-brand" style="font-family:Georgia, 'Times New Roman', Times, serif ; font-weight:bolder; color:red;">ISSACK TRAVELS</a>
                <button type="button" class="navbar-toggler"
                data-bs-toggle="collapse" data-bs-target="#navbarCollapse">
                <span class="navbar-toggler-icon"></span>
                </button>
                <div class="collapse navbar-collapse justify-content-between"
                id="navbarCollapse">
                <div class="navbar-nav">
                <a href="/" class="nav-item nav-link active" style="font-family:Georgia, 'Times New Roman', Times, serif ; font-weight:bolder; color:red;">HOME</a>
                <a href="/viewbookings" class="nav-item nav-link">MY BOOKINGS</a>
                
                <a href="contact.html" class="nav-item nav-link">CONTACT</a>
           
                </div>
                <form class="d-flex">
                
                <div class="input-group">
                <input type="text"
                class="form-control" placeholder="Search">
                <button type="button"
                class="btn btn-secondary">
                <i class="bi-search"></i>
                </button>
                </div>
                </form>
                <div class="navbar-nav">
                <a href="#" class="nav-item nav-link">Login</a>
                </div>
                </div>
                </div>
                </nav>
    
    
    
    
    
    
    
    
        </div>
        </section>
    <hr style="border: solid 4px red;">
   <h2 class="text-center" style="font-family:'Times New Roman', Times, serif; font-weight:bolder; color:red;">CHOOSE A BOOKING</h2>
   <hr style="border: solid 4px red;"><br>
   <section class="row container-fluid">
    <h2 class="text-center" style="font-family:'Times New Roman', Times, serif; font-weight:bolder; color:red;">{{msg}}</h2>

    {% for row in rows %}
    <div class="col-md-4">
        <div class="card shadow p-3">
           <img src="{{row[3]}}" alt="">
            <div class="card-body text-center">
                <h5 class="card-title">{{row[1]}}</h5>
                <p class="card-title">Reg Number: {{row[0]}}</p>
                <p class="card-text text-muted">  {{row[4]}}</p>
                <p class="card-text">KSH: {{row[5]}}</p>
                <!-- Button trigger modal -->
                <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal">
                Launch demo modal
                </button>

                
                
            </div>
           
        </div>
   </div><br>
   


{% endfor %}
   </section>
<br>
   
</body>
</html>
```
You loop one row in the many rows you have received and bind each row in the card using jinja templating language. You can go and read more about it. for example 
{{row[0]}} is reg date hence it will attach the reg date in the database there

As you can see our button has a modal id which when we press it will pop up a modal where we will put our phone number and amount so that we can pay.The form has the route to mpesa where when you press pay it will send you to the mpesa route and trigger and mpesa STK PUSH

Add the modal code after the section ends or closes
```

<!-- Modal -->
<div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
            <form action="/mpesa" method='post'>
          <input type="text" name="phone" class="form-control" placeholder="Enter Phone 2547xxxx"><br>
          <input type="text" name="amount" class="form-control" placeholder="Amount"><br>
          <button type="submit" class="btn btn-dark">PAY</button>
        </form>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
          
        </div>
      </div>
    </div>
  </div>

 
```
## STEP FOUR
Mpesa Route
Go to your python file and paste this route
```
import datetime
import base64
from requests.auth import HTTPBasicAuth
@app.route('/mpesa', methods = ['POST','GET'])
def mpesa():
        if request.method == 'POST':
            phone = str(request.form['phone'])
            amount = str(request.form['amount'])
            # GENERATING THE ACCESS TOKEN
            consumer_key = "GTWADFxIpUfDoNikNGqq1C3023evM6UH"
            consumer_secret = "amFbAoUByPV2rM5A"

            api_URL = "https://sandbox.safaricom.co.ke/oauth/v1/generate?grant_type=client_credentials" #AUTH URL
            r = requests.get(api_URL, auth=HTTPBasicAuth(consumer_key, consumer_secret))

            data = r.json()
            access_token = "Bearer" + ' ' + data['access_token']

            #  GETTING THE PASSWORD
            timestamp = datetime.datetime.today().strftime('%Y%m%d%H%M%S')
            passkey = 'bfb279f9aa9bdbcf158e97dd71a467cd2e0c893059b10f78e6b72ada1ed2c919'
            business_short_code = "174379"
            data = business_short_code + passkey + timestamp
            encoded = base64.b64encode(data.encode())
            password = encoded.decode('utf-8')


            # BODY OR PAYLOAD
            payload = {
                "BusinessShortCode": "174379",
                "Password": "{}".format(password),
                "Timestamp": "{}".format(timestamp),
                "TransactionType": "CustomerPayBillOnline",
                "Amount": amount,  # use 1 when testing
                "PartyA": phone,  # change to your number
                "PartyB": "174379",
                "PhoneNumber": phone,
                "CallBackURL": "https://modcom.co.ke/job/confirmation.php",
                "AccountReference": "account",
                "TransactionDesc": "account"
            }

            # POPULAING THE HTTP HEADER
            headers = {
                "Authorization": access_token,
                "Content-Type": "application/json"
            }

            url = "https://sandbox.safaricom.co.ke/mpesa/stkpush/v1/processrequest" #C2B URL

            response = requests.post(url, json=payload, headers=headers)
            print (response.text)
            return render_template('payment.html', msg = 'Please Complete Payment in Your Phone')
        else:
            return render_template('payment.html')

```
The folowing code recieves the phone and amount and triggers an stk push. If successfull it will render you to payment.html and an stk push will appear in your phone where you are asked to put the pin

## Last step
payment.html
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/css/bootstrap.min.css" integrity="sha384-B0vP5xmATw1+K9KRQjQERJvTumQW0nPEzvF6L/Z6nronJ3oUOFUFpCjEUQouq2+l" crossorigin="anonymous">
</head>
<body>

<div class="container">
<h1 class="jumbotron">My Grocery</h1>
<img src="https://post.healthline.com/wp-content/uploads/2020/09/Do_Apples_Affect_Diabetes_and_Blood_Sugar_Levels-732x549-thumbnail-1-732x549.jpg" alt="" class="img-fluid" width="30%">
    <h3>Red Apple</h3>
    <form action="/mpesa_payment" method="post">
        <input type="text" name="phone" placeholder="Enter Phone"> <br><br>
        <label for="">25 KES</label><br>
        <input type="hidden" name="amount" value="20" placeholder="Amount"><br>
        <input type="submit" value="Pay Now" class="btn btn-success">
    </form>
</div>
</body>
</html>


```
