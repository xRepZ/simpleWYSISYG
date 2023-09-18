<template>
  <div class="wrapper mt-77">
    <div class="d-flex ai-center">
      <button class="b-img" @click="undo()">
        <img src="./assets/editButtons/undo.svg" alt="отменить действие">
      </button>
      <button class="b-img ml-12" @click="redo()">
        <img src="./assets/editButtons/redo.svg" alt="повторить">
      </button>
      <button class="b-img ml-12" @click="wrapByTag('h1')">
        <img src="./assets/editButtons/head.svg" alt="заголовок">
      </button>
      <button class="b-img ml-12" @click="wrapByTag('p')">
        <img src="./assets/editButtons/paragraph.svg" alt="абзац">
      </button>
      <button class="b-img ml-12" @click="insertPicrure()">
        <img src="./assets/editButtons/image.svg" alt="картинка">
      </button>
      <button class="b-text ml-12" @click="copyHtml()">
        Скопировать HTML
      </button>
    </div>
    <!-- <div contenteditable="true" class="text-color text-font mt-31"> -->
    <div contenteditable="true" class="text-font text-color mt-31" ref="editableArea" @input="saveState()">
      Таким образом консультация с широким активом представляет собой интересный эксперимент проверки позиций, занимаемых участниками в отношении поставленных задач. С другой стороны постоянное информационно-пропагандистское обеспечение нашей деятельности представляет собой интересный эксперимент проверки форм развития. Идейные соображения высшего порядка, а также укрепление и развитие структуры влечет за собой процесс внедрения и модернизации соответствующий условий активизации. Задача организации, в особенности же реализация намеченных плановых заданий играет важную роль в формировании дальнейших направлений развития. Повседневная практика показывает, что постоянное информационно-пропагандистское обеспечение нашей деятельности играет важную роль в формировании существенных финансовых и административных условий.
      <h1>Смотрите какие обезьянки</h1>
      <img src="./assets/image/defImage.png" alt="monkey" class="img">
      <p>Таким образом консультация с широким активом представляет собой интересный эксперимент проверки позиций, занимаемых участниками в отношении поставленных задач. С другой стороны постоянное информационно-пропагандистское обеспечение нашей деятельности представляет собой инйцу шо шщйоц ущойц ущошцщйуо йцщуо йщцоу щйоу шщйцош ущйтересный эксперимент проверки форм развития. Идейные соображения высшего порядка, а также укрепление и развитие структуры влечет за собой процесс внедрения и модернизации соответствующий условий активизации. Задача организации, в особенности же реализация намеченных плановых заданий играет важную роль в формировании дальнейших направлений развития. Повседневная практика показывает, что постоянное информационно-пропагандистское обеспечение нашей деятельности играет важную роль в формировании существенных финансовых и административных условий.</p>
      <p>Товарищи! новая модель организационной деятельности требуют от нас анализа направлений прогрессивного развития. Задача организации, в особенности же постоянный количественный рост и сфера нашей активности требуют от нас анализа позиций, занимаемых участниками в отношении поставленных задач. Задача организации, в особенности же реализация намеченных плановых заданий требуют от нас анализа системы обучения кадров, соответствует насущным потребностям.</p>
    </div>
  </div>
</template>


<script setup>
import { onMounted, ref } from 'vue'

const editableArea = ref(null)

const history = []
let i = -1

const saveState = () => {
  i++
  history.splice(i, history.length - i, editableArea.value.innerHTML)
}
const restoreState = () => {
  editableArea.value.innerHTML = history[i]
}

onMounted(saveState)

const undo = () => {
  if (i > 0) {
    i--
    restoreState()
  }
}

const redo = () => {
  if (i + 1 < history.length) {
    i++
    restoreState()
  }
}

const wrapByTag = (tag) => {
  const sel = window.getSelection()
  if (!sel || sel.rangeCount === 0) return
  const range = sel.getRangeAt(0)
  if (editableArea.value !== range.commonAncestorContainer && !editableArea.value.contains(range.commonAncestorContainer)) return
  range.surroundContents(document.createElement(tag))
  sel.removeAllRanges()
  sel.addRange(range)
  saveState()
}

const insertPicrure = () => {
  const range = window.getSelection()?.getRangeAt(0)
  if (!range || range.startOffset !== range.endOffset) return
  if (editableArea.value !== range.commonAncestorContainer && !editableArea.value.contains(range.commonAncestorContainer)) return
  const img = document.createElement('img')
  img.src = window.prompt()
  img.className = "img"
  range.insertNode(img)
  range.setStartAfter(editableArea.value)
  saveState()
}

const copyHtml = () => {
  const html = `<html><body>${editableArea.value.innerHTML}</body></html>`
  navigator.clipboard.writeText(html)
}

</script>
