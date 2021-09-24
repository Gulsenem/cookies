<template>
    <div class=" wrapper">
        <div v-if="schritt==1">
            <h2>Anmelden</h2>
            <div class="container column" >
                <input type="text" placeholder="Benutzername" v-model="bName" id="bn">
                <input type="email" placeholder="Email" v-model="mail">
                <input type="password" placeholder="Passwort" v-model="pass">

                <button @click="login">Login</button>

                <div :class="meldung_class" > {{ meldung}} </div>
            </div> 
        </div>
        


        <div v-else-if="schritt==2"> 
            <h2>Herzlich Wilkommen!</h2>

            <button @click="hinzufugen('Iphone 5', 300)">Iphone 5, 300€</button>
            <button @click="hinzufugen('Iphone 6', 300)">Iphone 6, 400€</button>
            <button @click="hinzufugen('Iphone 7', 300)">Iphone 7, 500€</button>

            <h4>Warenkorb</h4>
            <ul>
                <li v-for="p in warenkorb" :key="p"> 
                        {{p.name}} - {{p.preis}}    
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
import Cookies from "js-cookie"

export default {
    created()
    {
       if (Cookies.get("logged_in", 1))
       {
           this.schritt ==2;

           if(Cookies.get("warenkorb"))
           {
               const liste = JSON.parse(Cookies.get("warenkorb"));
               for (let p of liste)
               {
                   this.warenkorb.push(p);
               }

           }
       }
    },
    mounted()
    {
        if(this.schritt==1)
        {
            document.getElementById("bn").focus();
        }
    },
    data()
    {
        return {
            bName: '',
            mail: '',
            pass: '',
            schritt: 1,
            meldung_class: "meldung versteckt",
            meldung: "Ungültige Anmeldedaten! Bitte versuchen Sie erneut.",
            warenkorb: [],


        }
    },

    methods:
    {
        login()
        {            
            if(this.bName=="Senem" && this.mail=="test@test.com" && this.pass=="123")
            {
                Cookies.set("logged_in", 1);
                this.schritt=2;
            }

            else 
            {
                
                this.meldung_class = "meldung";

                setTimeout(()=> {this.meldung_class = "meldung versteckt"}, 4000)
                
            }

        },

        hinzufugen(produkt, preis)
        {
            this.warenkorb.push({name:produkt, preis});

            Cookies.set("warenkorb", JSON.stringify(this.warenkorb));
            
        }

    }
}
</script>

<style scoped>
h2{margin:0;}
.container
{
    width: 300px;
    margin: 0 auto;
    margin-top: 30px;
}
.column
{
    display: flex;
    flex-direction: column;
}

.container input
{
    outline:none;
    border:none;
    margin-bottom: 12px;
    padding: 8px 12px;
    border-bottom: 1px solid #6e6e6e;
    font-size: 1em;
    transition: border 0.2s;
}

.container input:focus
{
    border-bottom: 2px solid #42b983;
}
button{
    margin: 0 20px;
    margin-top: 30px;
}
.container>button
{ 
    outline:none;
    border:none;
    cursor: pointer;
    padding: 8px 0;
    background-color:#42b983;
    color:white;
    font-weight:bold;
    font-size: 1em;
    margin-top: 30px;
    border-radius: 6px;
}
.container>button:hover
{
    background-color:#389b6e;
}

.meldung
{
    padding: 15px;
    margin-top: 20px;
    -webkit-box-shadow: -3px 6px 8px 0px rgba(0,0,0,0.3); 
    box-shadow: -3px 6px 6px 0px rgba(0,0,0,0.3);
    border-left: 10px solid crimson;
    border-radius: 6px;
    
    opacity: 1;
    transition: opacity 0.5s;
    
}
.meldung.versteckt
{
    opacity: 0;
}
</style>