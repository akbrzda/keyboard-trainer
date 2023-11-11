<script setup lang="ts">
import { ref, onMounted } from "vue";
onMounted(async () => {
     try {
          const response = await fetch('https://fish-text.ru/get?format=html&number=1');
          if (response.ok) {
               const textHTML = await response.text();
               const textContainer = document.getElementById('text-container');
               if (textContainer) {
                    textContainer.innerHTML = textHTML.toLowerCase();
                    var withoutTags = textHTML.replace(/<\/?[^>]+(>|$)/g, "");
                    var letters = withoutTags.split('');
                    var lettersSpan = letters.map(letter => `<span class="word">${letter}</span>`);
                    var span = lettersSpan.join('');
                    textContainer.innerHTML = span;

                    var errorCount = 0;
                    var currentLetterIndex = 0;

                    document.addEventListener('keydown', function (event) {
                         var key = event.key;
                         var currentLetter = letters[currentLetterIndex];
                         var wordElement = document.querySelector(".word:nth-child(" + (currentLetterIndex + 1) + ")");

                         if (currentLetter === key) {
                              wordElement.style.background = 'green';
                               wordElement.style.color = 'white';
                         } else {
                              wordElement.style.background = 'red'; 
                              wordElement.style.color = 'white';
                              errorCount++;
                         }

                         currentLetterIndex++;

                         if (currentLetterIndex === letters.length) {
                              textContainer.innerHTML = "Количество ошибок " + errorCount;
                              currentLetterIndex = 0;
                         }
                    });

                    document.addEventListener('keyup', function (event) {
                         console.log('Key Up: ' + event.key);
                    });
               }
          } else {
               console.error('Ошибка при загрузке текста:', response.status);
          }
     } catch (error) {
          console.error('Произошла ошибка:', error);
     }
});



</script>

<template>
     <div id="main">
          <div id="text-container"></div>
     </div>
</template>
<style scoped>
* {
     box-sizing: border-box;
}
.word.success {
    color: green;
}
.word.error {
    color: red;
}
</style>