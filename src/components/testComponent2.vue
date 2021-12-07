<template>
  <div class="counter">
    <textarea v-model="message" placeholder="add multiple lines"></textarea>
    <br />
    <button @click="updateData">Update</button>
    <input type="file" ref="doc" @change="readFile" />
    <br/>
    <span>Output:</span>
    <p style="white-space: pre-line;">{{ output }}</p>
    
  </div>
</template>

<script>

export default {
  name: 'testComponent2',
  data: function() {
    return {
      message: "",
      output: ""
    }
  },
  methods:
  {
    readFile: function () {
      console.log("read")
      this.file = this.$refs.doc.files[0];
      const reader = new FileReader();
      if (this.file.name.includes(".txt")) {
        reader.onload = (res) => {
          this.message = res.target.result;
        };
        reader.onerror = (err) => console.log(err);
        reader.readAsText(this.file);
      } else {
        reader.onload = (res) => {
          this.message(res.target.result);
        };
        reader.onerror = (err) => console.log(err);
        reader.readAsText(this.file);
      }
    },

    updateData: function () {
      if(!this.message)
        this.readFile();
      
     var initFish = this.message.split(',');
     var fishBornToday = []
     initFish.forEach((startFish) => {scheduleFish(fishBornToday,parseInt( startFish),1)});
     var fish = initFish.length
      for (let day = 0; day < 256; day++) {
          if(fishBornToday[day])
          {
            fish += fishBornToday[day]
            //+1 because i havent actually finished the day yet
            //new fish
            scheduleFish(fishBornToday,day+8+1,fishBornToday[day])
            //next litter
            scheduleFish(fishBornToday,day+6+1,fishBornToday[day])
          
          }
     }

    
    console.log("update done " + fish)
 
    }
  }
}
    function scheduleFish(birthSchedule,day,count)
    {
      if(birthSchedule[day])
        birthSchedule[day]+=count;
      else
      {
        birthSchedule[day] = count;
      }
    }
/*
function fishLife(day,daystillnewfish,name)
{
  console.log(name + " days to next spawn " + daystillnewfish)
  var spawnedFish = 0;
  for (var index = day; index < 5; index++) {
    if(daystillnewfish === 0)
    {
      spawnedFish+=fishLife(index,8,name+"nextGen");
      
      spawnedFish++;
      daystillnewfish = 6;
    }
    else{
  daystillnewfish--;
  
    }
  console.log(name +" day " + index + "spawn in " + daystillnewfish)
  
  }
  
  console.log(name + " spawned " + spawnedFish + "days left" + daystillnewfish)
  return spawnedFish;
}
*/
</script>

