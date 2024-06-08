<script>
    // @ts-nocheck
    let inp = "";

    let outputs;
    
    const months = ["january", "february", "march", "april", "may", "june", "july", "august", "september", "october", "november", "december"];
    const monthsabbr = ["jan", "feb", "mar", "apr", "may", "jun", "jul", "aug", "sep", "sept", "oct", "nov", "dec"];

    const days = ["sunday", "monday", "tuesday", "wednesday", "thursday", "friday", "saturday", "sundays", "mondays", "tuesdays", "wednesdays", "thursdays", "fridays", "saturdays"];
    const daysabbr = ["sun", "mon", "tue", "tues", "wed", "thu", "thur", "thurs", "fri", "sat"];

    class FinalDate {
        month = [];
        date = [];
        day = [];
        year = [];
        time = [];
        constructor() {}

        getter() {
            let start = "";
            let end = "";
            start += this.month[0] + " ";
            start += this.date[0] + " ";
            start += this.year[0] + " ";
            let stime = "";
            stime += parseInt(this.time[0] / 60);
            stime += ":";
            stime += this.time[0] % 60;
            start += stime;
            if (this.time[0] % 60 < 10) {
                stime += "0";
            }
            
            end += this.month[1] + " ";
            end += this.date[1] + " ";
            end += this.year[1] + " ";
            let etime = "";
            etime += parseInt(this.time[1] / 60);
            etime += ":";
            etime += this.time[1] % 60;
            if (this.time[1] % 60 < 10) {
                etime += "0";
            }
            end += etime;

            console.log("Formatted Times << " + stime + " to " + etime)
            
            console.log("Date Strings << [" + start + "], " + "[" + end + "]")

            return [new Date(start), new Date(end), this.day];
        }
    }

    let finaloutput; 

    function handleclick(){

        finaloutput = new FinalDate();

        inp.replaceAll(",", "");
        let text = inp.toLowerCase().split(" ");
        console.log("Input >> " + text);
        for (let i = 0; i < text.length; i++){
            let each = text[i];
            // find months, days
            if (months.indexOf(each) >= 0){
                finaloutput.month = [...finaloutput.month, months[months.indexOf(each)]];
            } else if (monthsabbr.indexOf(each) >= 0) {
                finaloutput.month = [...finaloutput.month, monthsabbr[monthsabbr.indexOf(each)]];
            } 
            else if (days.indexOf(each) >= 0){
                finaloutput.day = [...finaloutput.day, days[days.indexOf(each) % 7]];
            } else if (daysabbr.indexOf(each) >= 0) {
                finaloutput.day = [...finaloutput.day, daysabbr[daysabbr.indexOf(each)]];

            } 
            else if (each === "am") {
                let thistime = text[i-1];
                finaloutput.time = [...finaloutput.time, determineTime(true, thistime)];
            } else if (each === "pm") {
                let thistime = text[i-1];
                finaloutput.time = [...finaloutput.time, determineTime(false, thistime)];
            } else if (each.includes("am")) {
                finaloutput.time = [...finaloutput.time, determineTime(true, each)];
            }else if (each.includes("pm")) {
                finaloutput.time = [...finaloutput.time, determineTime(false, each)];
            }
            else if (each.length === 4 && parseInt(each) >= new Date().getFullYear()){
                finaloutput.year = [...finaloutput.year, each];
            } 
            else if (months.indexOf(text[i-1]) >= 0 || monthsabbr.indexOf(text[i-1]) >= 0 || months.indexOf(text[i+1]) >= 0 || months.indexOf(text[i+1]) >= 0) {
                if (parseInt(each) <= 31) {
                    finaloutput.date = [...finaloutput.date, each];
                }
            }
        }

        if (finaloutput.year.length < 2){
            finaloutput.year = [...finaloutput.year, new Date().getFullYear()];
        }
        if (parseInt(finaloutput.year[1]) < parseInt(finaloutput.year[0])) {
            finaloutput.year = [finaloutput.year[1], finaloutput.year[0]];
        }
        if (parseInt(finaloutput.time[1]) < parseInt(finaloutput.time[0])) {
            finaloutput.year = [finaloutput.time[1], finaloutput.time[0]];
        }

        // debug outputs 
        console.log("Years << " + finaloutput.year); 
        console.log("Months << " + finaloutput.month);
        console.log("Dates << " + finaloutput.date); // not complete
        console.log("Days << " + finaloutput.day);
        console.log("Times << " + finaloutput.time);

        outputs = finaloutput.getter();
        console.log("Outputs << " + outputs);
    }

    function determineTime(isam, thistime){
        // thistime:string
        // am:boolean
        let timeInMinutes;
    
        if (isam){
            timeInMinutes = 0;
        } else {
            timeInMinutes = 12*60;
        }
    
        if (thistime.indexOf(":") > 0){
            let x = thistime.split(":");
            x[0] = parseInt(x[0]);
            x[1] = parseInt(x[1]);
            timeInMinutes += x[0]*60 + x[1];
        } else {
            timeInMinutes += parseInt(thistime)*60;
        }
        return timeInMinutes;
    }

</script>

<main>

    <input id="input" type="text" bind:value={inp} placeholder="May 4 2024 to May 4 2025 from 10 AM to 4 PM on Tuesdays">
    <input id="btn" type="button" on:click={handleclick} value="Generate">
    <br>

</main>

<style>
    main {
        width: 100%;
        font-family: Calibri, sans-serif;
    }

    #input {
        padding: 20px;
        width:500px;
        height:50px;
        font-size: large;
        border-top: whitesmoke;
        border-left: whitesmoke;
        border-right: whitesmoke;
    }

    #btn {
        width:200px;
        height:91px;
        font-size: larger;
        background-color: #4aa4e9;
        color:whitesmoke;
        border:whitesmoke;
    }
</style>