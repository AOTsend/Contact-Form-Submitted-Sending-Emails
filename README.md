# Submit Contact Form Auto Sending Emails (PHP Version)
Submit a Contact Form will Auto Sending Emails to Predetermined Email Address.

The AOTsend Contact Form is a PHP file source code designed for a specific purpose: when a user inputs information into the form and clicks submit, the PHP file automatically sends the form data via email to a designated client email address.

.

.

## Register AOTsend to Get app_key and template_id


1.Register an account on [AOTsend Email API Platform](https://www.aotsend.com).

2.Bind your own domain and complete domain verification. Once verified, it will automatically submit for review. Wait for the review to be approved.

3.Set up an email template. AOTsend provides some high-quality, minimalist templates that can be adapted for various uses.

4.Obtain the app_key and template_id from steps 2 and 3. (If unclear, contact AOTsend support.)

5.Download the PHP file (source code for automatic email sending via Contact Form).

6.On line 31 of the PHP file, replace app_key and template_id with your own. Also, specify the email address where you want to receive the emails.

7.Place the PHP file in your website directory and access it via URL.

8.Submit a test form to ensure emails are received in your specified mailbox. Remember checking your spam folder if using a new domain.


.
.


## Contact Form Auto Sending Emails via AOTsend API (PHP Version)

        <?php
        if(!empty($_POST['is_post']) && $_POST['is_post']==1){
            $url = "https://www.aotsend.com/index/api/send_email";
            $name = $_POST['name'];
            $email = $_POST['email'];
            $subject = $_POST['subject'];
            $message = $_POST['message'];
            if(empty($name)){
                echo json_encode(['message'=>'Please Enter Your Name','code' => 40001]);
                exit;
            }
            if(empty($email)){
                echo json_encode(['message'=>'Please Enter Email address','code' => 40002]);
                exit;
            }
            if(empty($subject)){
                echo json_encode(['message'=>'Please Enter Subject','code' => 40003]);
                exit;
            }
            if(empty($message)){
                echo json_encode(['message'=>'Please Enter Message','code' => 40004]);
                exit;
            }
            $time = date('Y-m-d H:i:s',time());
        
            $str = '{"username":"'.$name.'","contactemail":"'.$email.'","subject":"'.$subject.'","content":"'.$message.'","time":"'.$time.'"}';
        
            //app_key (Register AOTsend to Obtain)
            //to (Generally Enter Your Email Address)
            //template_id (Email Template ID in AOTsend)
            $data = ['app_key'=>'cf6d0114ee5cd1e4800000005c20ac793', 'to'=>'test@aotsend.com', 'template_id'=>'E_1000000004408', 'data'=>$str];
        
            $curl = curl_init();
            curl_setopt($curl, CURLOPT_URL, $url);
            curl_setopt($curl, CURLOPT_SSL_VERIFYPEER, FALSE);
            curl_setopt($curl, CURLOPT_SSL_VERIFYHOST, FALSE);
            if (!empty($data)){
                curl_setopt($curl, CURLOPT_POST, 1);
                curl_setopt($curl, CURLOPT_POSTFIELDS, $data);
            }
            curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1);
            $output = curl_exec($curl);
            curl_close($curl);
            echo $output;
            exit;
        }
        ?>
        <style type="text/css">
        html, body {
          background: #f1f1f1;
          font-family: 'Merriweather', sans-serif;
          padding: 1em;
        }
        
        h1 {
           text-align: center;
           color: #565656;
           @include text-shadow(1px 1px 0 rgba(white, 1));
        }
        p{
          text-align: center;
        }
        form {
           max-width: 600px;
           text-align: center;
           margin: 20px auto;
          
          input, textarea {
             border:0; outline:0;
             padding: 1em;
             @include border-radius(8px);
             display: block;
             width: 100%;
             margin-top: 1em;
             font-family: 'Merriweather', sans-serif;
             @include box-shadow(0 1px 1px rgba(black, 0.1));
             resize: none;
            
            &:focus {
               @include box-shadow(0 0px 2px rgba($red, 1)!important);
            }
          }
          
          #input-submit {
             color: white; 
             background-color: #ff5151;
             cursor: pointer;
             margin-top:20px;
            
            &:hover {
               @include box-shadow(0 1px 1px 1px rgba(#aaa, 0.6)); 
            }
          }
          
          textarea {
              height: 156px;
          }
        }
        
        
        .half {
          float: left;
          width: 48%;
          margin-bottom: 1em;
        }
        
        .right { width: 50%; }
        
        .left {
             margin-right: 2%; 
        }
        
        
        @media (max-width: 480px) {
          .half {
             width: 100%; 
             float: none;
             margin-bottom: 0; 
          }
        }
        
        
        /* Clearfix */
        .cf:before,
        .cf:after {
            content: " "; /* 1 */
            display: table; /* 2 */
        }
        
        .cf:after {
            clear: both;
        }
        </style>
        <h1>Contact Form</h1>
        <p>Supported by AOTsend Email API</p>
        <form class="cf">
          <div class="half left cf">
            <input type="text" id="input-name" placeholder="Name">
            <input type="email" id="input-email" placeholder="Email address">
            <input type="text" id="input-subject" placeholder="Subject">
          </div>
          <div class="half right cf">
            <textarea name="message" type="text" id="input-message" placeholder="Message"></textarea>
          </div>  
          <input type="submit" value="Submit" id="input-submit">
        </form>
        
        <script>
          function submitForm() {
            // Prevent the default form submission behavior
            event.preventDefault();
        
            // Assuming your form data is in the following object
            var formData = {
              is_post: 1,
              name: document.getElementById('input-name').value,
              email: document.getElementById('input-email').value,
              subject: document.getElementById('input-subject').value,
              message: document.getElementById('input-message').value
            };
        
            // Convert the form data into a query string
            var queryString = Object.keys(formData).map(key => encodeURIComponent(key) + '=' + encodeURIComponent(formData[key])).join('&');
        
            // Initialize the XMLHttpRequest object
            var xhr = new XMLHttpRequest();
        
            // Set the request type, URL, and asynchronous flag
            xhr.open('POST', '', true);
        
            // Set the request headers (if needed)
            xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
        
            // Set the response handling function
            xhr.onreadystatechange = function () {
              if (xhr.readyState === 4 && xhr.status === 200) {
                // Request completed successfully
                var obj = JSON.parse(xhr.responseText);
                if(obj.code==200){
                  //Call was successful
                  alert("Submission successful; email has been sent!")
                }else{
                  alert(obj.message)
                }
              }
            };
            // Send the request
            xhr.send(queryString);
          }
          document.getElementById('input-submit').addEventListener('click', submitForm);
        </script>
