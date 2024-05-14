


const idFields = ['username','firstname','lastname','phone','email','fax']
const names = ['Username','First Name','Last Name','Phone','Email','Fax number']
$('#reset').on('click',function(){
  for (let i = 0; i < idFields.length; i++) {
    document.getElementById(idFields[i]).value = "";
    document.getElementById(idFields[i]).parentElement.classList.remove('has-error');
  }
  document.getElementById("adults").value = 1;
  document.getElementById("range").value = 5;
  document.getElementById("checkin").value = "";
  document.getElementById("checkout").value = "";
  document.getElementById("days").value = "";
  document.getElementById("cost").value = "";
  document.getElementById("message").value = "";
  document.getElementById("low").checked = true;  
  toastr.info("The fields were successfully cleared");
  })
 
$('#submit').on('click', function() {
  var isValid = true;
    
  if(document.getElementById('cost').value < 0 && document.getElementById('checkin').value!="" && document.getElementById('checkout').value!=""){
    isValid = false;
    toastr.error("cost is negative, please enter valid checkin/checkout dates");    
  }
  

    
  if(isNaN(parseFloat(document.getElementById('cost').value)) ||document.getElementById('cost').value == 0 ){
    isValid = false;
    toastr.error("No cost was calculated, please enter valid checkin/checkout dates");
    document.getElementById('days').value = "";
    document.getElementById('cost').value = "";    
  }
      
  
  var missingFlag = false;
    for (let i = 0; i < idFields.length; i++){
    if(document.getElementById(idFields[i]).value == ""){
      isValid = false;     
      document.getElementById(idFields[i]).parentElement.classList.add('has-error');
      
      toastr.error("Please fill " + names[i])
      
    } 
    else{
       document.getElementById(idFields[i]).parentElement.classList.remove('has-error');
      
    }
   }
   
 
  if(isValid){
      toastr.success("The form was successfully submitted");
  }
  });
  

$('#adults').on('change',function(){
  if(document.getElementById('checkin')!="" &&
     document.getElementById('checkout')!=""){
    document.getElementById('cost').value=document.getElementById('days').value*150*document.getElementById('adults').value;
  }
})
                
                
$('#checkin').on('change',function(){
  if(document.getElementById('checkout')!=""){
      var date1 = moment (document.getElementById("checkin").value);
      var date2 = moment(document.getElementById("checkout").value);
      var adults = document.getElementById("adults").value;
      if (date1 && date2 && adults) {
        var difference = date2.diff(date1, 'days');
        document.getElementById("days").value = difference;
                      document.getElementById('cost').value= difference*150*adults;
     }
  }
})
  

  

$('#checkout').on('change',function(){
  if(document.getElementById('checkin')!=""){
      var date1 = moment (document.getElementById("checkin").value);
      var date2 = moment(document.getElementById("checkout").value);
      var adults = document.getElementById("adults").value;
      if (date1 && date2 && adults) {
        var difference = date2.diff(date1, 'days');
        document.getElementById("days").value = difference;
        document.getElementById('cost').value=difference*150*adults;
      }
  }
})
 
