<template>
  <div class="container">
    <div class="wrapper">

    
     <div class="btnsContainer">

       <button @click="undo">
          <img src="@/assets/undo.svg" alt="undoIcon" class="icon" loading="lazy">
       </button>
       <button @click="redo">
          <img src="@/assets/redo.svg" alt="redoIon" class="icon" loading="lazy">
       </button>
       <button @click="makeHeading">
          <img src="@/assets/upperCase.svg" alt="upperCaseIcon" class="icon" loading="lazy">
       </button>
       <button  class="lowerCaseBtn" @click="makeParagraph" ref="makeParagraph">
          <img src="@/assets/lowerCase.svg" alt="lowerCaseIcon" class="icon" loading="lazy">
          <img src="@/assets/lowerCase.svg" alt="lowerCaseIcon" class="icon" loading="lazy">
       </button>
       <button @click="openImagePrompt">
          <img src="@/assets/image.svg" alt="imageIcon" class="icon" loading="lazy">
       </button>
       <span @click="copyHTML">Копировать HTML</span>

      </div>

        <div contenteditable="true" ref="editor" class="editor" @mouseup="handleTextSelection" @keyup="handleTextSelection" @input="handleInput">
          <p>Таким образом консультация с широким активом представляет собой интересный эксперимент проверки позиций, занимаемых участниками в отношении поставленных задач.
             С другой стороны постоянное информационно-пропагандистское обеспечение нашей деятельности представляет собой интересный эксперимент проверки форм развития.
              Идейные соображения высшего порядка, а также укрепление и развитие структуры влечет за собой процесс внедрения и модернизации соответствующий условий активизации.
              Задача организации, в особенности же реализация намеченных плановых заданий играет важную роль в формировании дальнейших направлений развития. Повседневная практика показывает,
             что постоянное информационно-пропагандистское обеспечение нашей деятельности играет важную роль в формировании существенных финансовых и административных условий.</p>
          <h1>Смотрите какие обезьянки</h1>
          <img src="@/assets/picDefault.svg" alt="Изображение по умолчанию" />
          
          <p>
            Таким образом консультация с широким активом представляет собой интересный эксперимент проверки позиций, занимаемых участниками в отношении поставленных задач.
            С другой стороны постоянное информационно-пропагандистское обеспечение нашей деятельности представляет собой инйцу шо шщйоц ущойц ущошцщйуо йцщуо йщцоу щйоу шщйцош 
            ущйтересный эксперимент проверки форм развития. Идейные соображения высшего порядка, а также укрепление и развитие структуры влечет за собой процесс внедрения и модернизации 
            соответствующий условий активизации. Задача организации, в особенности же реализация намеченных плановых заданий играет важную роль в формировании дальнейших направлений развития.
            Повседневная практика показывает, что постоянное информационно-пропагандистское обеспечение нашей деятельности играет важную роль в формировании существенных финансовых и 
            административных условий.
          </p>
          <p>
            Товарищи! новая модель организационной деятельности требуют от нас анализа направлений прогрессивного развития. Задача организации, в особенности же постоянный 
            количественный рост и сфера нашей активности требуют от нас анализа позиций, занимаемых участниками в отношении поставленных задач. Задача организации, в особенности же
             реализация намеченных плановых заданий требуют от нас анализа системы обучения кадров, соответствует насущным потребностям.
          </p>
         
        </div>
      
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    handleTextSelection() {
      const selection = window.getSelection(); // выделенная область 
      const selectedText = selection.toString(); // выделенный текст
      this.selectedText = selectedText; // кидаем в локальный стейт выделенный текст
      const selectedElement = selection.focusNode.parentElement; // DOM-элемент, на котором произошло выделение текст
      this.start =  window.getSelection().getRangeAt(0).startOffset; // начальный индекс в текущей ноде
      this.end =  window.getSelection().getRangeAt(0).endOffset; // конечный индекс в текущей ноде
      if (selectedElement) {
          this.selectedElement = selectedElement; // сохранение DOM-элемента, на котором произошло выделение текст    
        }
    },
    makeHeading() {
      if(!this.selectedText){
        return
      }
      const selection = window.getSelection(); // выделенная область 
      const editor = this.$refs.editor; // Получение ссылки на редактор
      const allElementsInsideEditor = Array.from(editor.querySelectorAll('*')); // Получаем массив всех DOM-элементов в редакторе 
      const selectedElement = selection.focusNode.parentElement; // DOM-элемент, на котором произошло выделение текст
      const currentElementIndex = allElementsInsideEditor.indexOf(selectedElement); // индекс текущего DOM-элемента

      if (selectedElement) {
          selectedElement.remove(); // удаляем из DOM-дерева элемент
          console.log(selectedElement)
          }
      
      let beforeText = this.selectedElement.textContent.substring(0, this.start); // текст до выделения
      let afterText = this.selectedElement.textContent.substring(this.end, this.selectedElement.textContent.length); // текст после выделения
      
      let headingText = `<h1>${this.selectedText}</h1>`; // выделенный текст делаем заголовком
      this.selectedElement.innerHTML = `<p>${beforeText}</p><br>${headingText}<br><p>${afterText}</p>`; // соединяем все три части
      allElementsInsideEditor.splice(currentElementIndex, 0, this.selectedElement); // кидаем обновленный DOM-элемент в массив всех элементов редактора 
      editor.innerHTML = ''; 
      allElementsInsideEditor.forEach(element => editor.appendChild(element)); // фаршируем результатом наш редактор 
      this.saveTextInHistory(); // шоб могли откатить
      this.resetParams() // обнуляем параметры 
    },
      makeParagraph() {
     
      if(!this.selectedText){
        return
      }
      const selection = window.getSelection(); // выделенная область 
      const editor = this.$refs.editor; // Получение ссылки на редактор
      const allElementsInsideEditor = Array.from(editor.querySelectorAll('*')); // Получаем массив всех DOM-элементов в редакторе 
      const selectedElement = selection.focusNode.parentElement; // DOM-элемент, на котором произошло выделение текст

      const currentElementIndex = allElementsInsideEditor.indexOf(selectedElement); // индекс текущего DOM-элемента
      console.log(currentElementIndex)
      if (selectedElement) {
        selectedElement.remove(); // удаляем из DOM-дерева элемент
      
      }
      let beforeText = this.selectedElement.textContent.substring(0, this.start); // текст до выделения
      let afterText = this.selectedElement.textContent.substring(this.end, this.selectedElement.textContent.length); // текст после выделения
      let firstLetter = this.selectedText.charAt(0).toUpperCase(); //  делаем первый символ абзаца заглавным
      let restOfText = this.selectedText.slice(1).toLowerCase(); // хз работает ли но приводим к нижнему регистру
      restOfText = restOfText.charAt(restOfText.length - 1) === "." ? restOfText.toLowerCase() : `${restOfText.toLowerCase()}.`; // если нет точки в конце - ставим
      const paragraphText = `<p>${firstLetter}${restOfText}</p>`; // окутываем в нужный тег
      this.selectedElement.innerHTML = `${beforeText}${paragraphText}${afterText}`; // соединяем все три части
      allElementsInsideEditor.splice(currentElementIndex, 0, this.selectedElement); // кидаем обновленный DOM-элемент в массив всех элементов редактора 
      editor.innerHTML = ''; 
      allElementsInsideEditor.forEach(element => editor.appendChild(element)); // фаршируем результатом наш редактор    
      this.saveTextInHistory(); // шоб могли откатить
      this.resetParams(); // обнуляем параметры

    },
    saveTextInHistory() {
      const currentText = this.$refs.editor.innerHTML;
      this.currentHtmlIndex = this.currentHtmlIndex + 1;
      this.textHistory.push(currentText);
    },
    undo() {
    if(this.currentHtmlIndex === 0){
      return
    }
    const editor = this.$refs.editor;
    let oldHtml = this.textHistory[this.currentHtmlIndex-1];
    this.currentHtmlIndex = this.currentHtmlIndex -1;
    editor.innerHTML = oldHtml;
    },
    redo() {
      if(this.currentHtmlIndex + 1 > this.textHistory.length - 1){
        return
      }
      const editor = this.$refs.editor;
      let newHtml = this.textHistory[this.currentHtmlIndex + 1];
      this.currentHtmlIndex = this.currentHtmlIndex + 1;
      editor.innerHTML = newHtml;
    },
    resetParams(){
      this.start = null;
      this.end = null;
      this.selectedElement = null;
      this.selectedText = "";
     
    },
    openImagePrompt() {
      const imageUrl = prompt("Введите URL изображения:");
      sessionStorage.setItem("imageUrl", imageUrl);
      if (imageUrl) {
       const editor = this.$refs.editor;
       // Добавляем обработчик события клика на редактор
       editor.addEventListener('click', this.handleEditorClick);
      }
    },
      handleEditorClick(event) {
      const editor = this.$refs.editor;
      const editorNodes = editor.querySelectorAll('*');

      // Проверяем, был ли клик выполнен на какой-либо из вложенных нод
      for (const node of editorNodes) {
      if (node === event.target || node.contains(event.target)) {
      // Клик был выполнен на тексте или внутри блока editor, не подгружаем фото
        return;
      }
      }

    // Если клик был выполнен вне текста и блока editor, открываем инпут для изображений
       const imageUrl = sessionStorage.getItem('imageUrl');
       if(imageUrl) {
       const editor = this.$refs.editor;
       const imgElement = document.createElement('img');
       imgElement.style.width = '70%';
       imgElement.style.display = 'block';
       imgElement.style.marginBottom = '25px';
       imgElement.src = imageUrl;
       editor.appendChild(imgElement);
       editor.removeEventListener('click', this.handleEditorClick);
       this.saveTextInHistory();
  }
  },
  copyHTML() {
  const imgElement = document.querySelectorAll('img'); 
  const imgUrl = imgElement.src ? imgElement.src : "";

    // Получаем текстовое содержимое редактора
    const editorContent = document.querySelector('.editor').innerHTML;

    // Собираем текст и изображение вместе в формате HTML
    const combinedContent = `<div>${editorContent}</div><img src="${imgUrl}" />`;

    // Создаем временный элемент textarea
    const tempElement = document.createElement('textarea');
    tempElement.value = combinedContent;
    document.body.appendChild(tempElement);

    // Выделяем текст в textarea
    tempElement.select();

    // Копируем текст и изображение в буфер обмена
    document.execCommand('copy');

    // Удаляем временный элемент
    document.body.removeChild(tempElement);
    },
    handleInput() {
      // Вызываем метод для сохранения истории при вводе текста
      this.saveTextInHistory();
    },
  },
  
  data() {
    return {
      selectedText: "", 
      textHistory: [], 
      currentHtmlIndex: -1,
      selectedElement : null,
      start : null,
      end : null,
    };
  },
  mounted() {
    this.saveTextInHistory();
  },
};
</script>

<style scoped>
.container{
  background-color : #1E1E1E;
  width: 100%;
  min-height: 100vh;
  display: flex;
  justify-content: center;
}

.wrapper{
  max-width: 1240px;
  /* border: solid red; */
  width: 100%;
  height: auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 77px 107px 107px 107px;
}


.btnsContainer{
  width: 100%;
  display: flex;
  justify-content: start;
  align-items: center;
}
.lowerCaseBtn .icon{
 width: 12px;
 height: 13px;
}

button{
  width: 42px;
  height: 38px;
  background-color: #282828;
  border-radius: 4px;
  padding: 5px;
  margin: 0 10px 0 0;
  border: none;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}
.icon{
  width: 21px;
  height: 20px;
}
span{
  font-size: 15px;
  font-weight: 400px;
  color : #639EFF;
  font-family: 'Roboto';
}
p{
  font-size: 15px;
  font-weight: 400px;
  color: white;
}
.editor{
  max-width: 1240px;
  height: 100%;
  color: white;
  margin: 50px 0;
  background-color: #282828;
  padding: 30px 50px;
  border-radius: 4px;
  border: #639EFF solid 1px;
}
.editor:hover {
  cursor: default;
}
.font-styled-text{
   font-size: 15px;
  font-weight: 400px;
  color: white;
}
img{
  width: 70%;
}
@media screen and (max-width : 767px) {
.wrapper{
  max-width: 100%;
}
.editor{
  width: 60%;
  min-height: 100vh;
}
}
</style>



