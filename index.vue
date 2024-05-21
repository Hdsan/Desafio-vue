<script setup>
import { reactive, onMounted } from "vue";
import "@melloware/coloris/dist/coloris.css";
import Coloris from "@melloware/coloris";
import { ref, watch } from 'vue';

let action = ref("none")
let only = ref("all")
let stock = ref("ever")
let tag = ref([])
let title = ref("");

let image = reactive({
  file: "",
  url: "#000000",
})

const defaultImage = "../assets/logo.png";
const types = [
  { title: "Texto", value: "title" }, { title: "Imagem", value: "image" }, { title: "Icone", value: "icon" }, { title: "Icone e Texto", value: "text_icon" }
]
const icons = [{ title: "Grande", value: "big" }, { title: "Médio", value: "mid" }, { title: "Pequeno", value: "small" },]

const tagPosition = [
  { title: "Dentro da Imagem", value: "insideImage" }, { title: "Acima da Imagem", value: "beforeImage" }, { title: "Acima do titulo", value: "beforeTitle" }, { title: "Após o preço", value: "afterPrice" }, { title: "Após o botão de compra", value: "afterBuy" }
]
const rounding = [
  { title: "Pouco Arredondado", value: "4px" }, { title: "Muito Arredondado", value: "30px" }, { title: "Quadrado", value: "0" }
]
const tagSize = [
  { title: "Minimo Possível", value: "Fit-content" }, { title: "Máximo Possível", value: "100%" }
]
const actions = [
  { value: 'edit', name: 'Editar' },
  { value: 'delete', name: 'Deletar' },
];
const visitors = ref([
  { name: 'Visitor 1', value: 'visitor1' },
  { name: 'Visitor 2', value: 'visitor2' },
]);


let editableText;
let background = reactive({
  color: "#f0f0f0",
  text: "#000000",
  icon: "#000000"
})
let border = reactive({
  thickness: 1,
  color: "#000000"
})
let recommended = reactive({
  size: false,
  format: false
})
let alignment = reactive({
  vertical: {
    x: 0,
    y: 0,
    z: 0
  },
  horizontal: {
    x: 0,
    y: 0,
    z: 0
  }
})
let fontSize = ref(13)

let wppNumber = ref("");
let message = ref("");
let link = ref("");

const editText = (text) => {
  editableText = text.target.textContent
  return editableText;
}

const onFileChange = (event) => {
  const file = event.target.files[0];
  if (file) {
    image.file = file
    image.url = URL.createObjectURL(file);

  }
};


function getDescription(action, type) {
  return type?.find(({ value }) => value === action)?.["description"] || "";
}

watch(image, () => {
  const file = image.file;
  let width;
  let height;
  if (file) {
    const img = new Image();
    img.onload = function () {
      width = this.naturalHeight;
      height = this.naturalWidth
      console.log(height, width, extension)
      if (width <= 200 && height <= 200) {
        recommended.size = true
      }
      else {
        recommended.size = false;
      }
      if (extension == "png" || extension == "jpg" || extension == "gif" || extension == "jpeg") {
        recommended.extension = true
      }
      else {
        recommended.extension = false
      }
    };
    img.src = URL.createObjectURL(file);
  }
  const extension = image.file.name.split('.').pop();
});


onMounted(async () => {

  Coloris.init();
  Coloris({
    el: ".coloris",
    theme: "pill",
    themeMode: "dark",
    formatToggle: true,
    closeButton: true,
    closeLabel: "Fechar",
  });

});


</script>

<template>
  <div class="container">
    <section class="settings">
      <ul class="nav nav-pills" id="pills-tab" role="tablist">
        <li class="nav-item" role="presentation">
          <button class="nav-link active" data-bs-toggle="pill" data-bs-target="#settings">
            Exibição
          </button>
        </li>
        <li class="nav-item" role="presentation">
          <button class="nav-link" data-bs-toggle="pill">
            Customização
          </button>
        </li>
        <li class="nav-item" role="presentation">
          <button class="nav-link" data-bs-toggle="pill">
            Ação
          </button>
        </li>
      </ul>
      <div class="tab-content" id="pills-tabContent">
        <div class="tab-pane fade show active" id="settings">
          <div class="inputs cScroll">
            <div class="input">
              <label>Titulo</label>
              <input v-model="title" type="text" placeholder="Ex: Novo Template" />
              <small> Identificação da tag internamente no sistema </small>
            </div>
            <hr />
            <div class="input">
              <label>Tipo</label>
              <select>
                <option v-for="({ title, value }, key) in types" :key="key" :value="value" selected>{{ title }}</option>
              </select>
            </div>
            <div class="input" data-field="change_icon">
              <label>Ícone</label>
              <select>
                <option :selected="key === 0" v-for="({ title, value }, key) in icons" :value="value" :key="key">
                  {{ title }}
                </option>
              </select>
            </div>
            <div class="input" data-field="change_text">
              <label>Texto da Tag</label>
              <div contenteditable="true" @input="editText">Exemplo</div>
            </div>
            <div class="input file" data-field="change_image" ref="change_image">
              <label>Imagem</label>
              <div>
                <span id="selected_image">
                  <div ref="exclude_image">
                    <i class="bx bx-trash"></i>
                  </div>
                  <img :src="image.url" :style="{ maxWidth: '250px', maxHeight: '250px' }" />
                </span>
                <label for="change_image">
                  <input @change="onFileChange" type="file" />
                  <i class="bx bx-images"></i>
                  Selecione uma Imagem
                </label>
                <ul>
                  <li :style="{ color: recommended.size ? 'green' : 'red' }">
                    <strong>Tamanho Recomendado:</strong>
                    200x200
                  </li>
                  <li :style="{ color: recommended.extension ? 'green' : 'red' }">
                    <strong>Formatos Aceitos:</strong>
                    png, jpg, gif e jpeg
                  </li>
                </ul>
              </div>
            </div>
          </div>
        </div>
        <div class="tab-pane fade" id="custom">
          <div class="inputs cScroll">
            <div class="input">
              <label>Posição da Tag</label>
              <select>
                <option :selected="key === 0" v-for="({ title, value }, key) in tagPosition" :value="value" :key="key">
                  {{ title }}
                </option>
              </select>
            </div>
            <div class="input" :data-field="'change_radius'">
              <label>Arredondamento</label>
              <select ref="rounded">
                <option :selected="key === 0" v-for="({ title, value }, key) in rounding" :value="value" :key="key">
                  {{ title }}
                </option>
              </select>
            </div>
            <div class="input" :data-field="'change_width'">
              <label>Tamanho da Tag</label>
              <select>
                <option :selected="key === 0" v-for="({ title, value }, key) in tagSize" :value="value" :key="key">
                  {{ title }}
                </option>
              </select>
            </div>
            <hr />
            <div class="input" ref="background_color" data-type="backgroundColor">
              <label>Cor do Fundo</label>
              <div class="input-picker">
                <input class="coloris" v-model=background.color />
              </div>
            </div>
            <div class="input" ref="text_color" data-type="color">
              <label>Cor do Texto</label>
              <div class="input-picker">
                <input class="coloris" v-model=background.text />
              </div>
            </div>
            <div class="input" ref="icon_color" data-type="fill" data-field="icon_color">
              <label>Cor do Ícone</label>
              <div class="input-picker">
                <input class="coloris" v-model=background.icon />
              </div>
            </div>
            <hr />
            <div class="input">
              <label>Tipo da Borda</label>
              <select ref="border_type" data-type="borderStyle">
                <option value="none" selected>Nenhuma</option>
                <option value="solid">Sólida</option>
                <option value="dashed">Tracejada</option>
                <option value="dotted">Pontilhada</option>
              </select>
            </div>
            <div class="input" ref="border_width" data-type="borderWidth">
              <span class="input-title">
                <label>Espessura da borda </label>
                <small>{{ border.thickness }}</small>
              </span>
              <input type="range" min="1" max="6" v-model=border.thickness />
            </div>
            <div class="input" ref="border_color" data-type="borderColor">
              <label>Cor da Borda</label>
              <div class="input-picker">
                <input class="coloris" v-model=border.color />
              </div>
            </div>
            <hr />
            <div class="input align" ref="align_horizontal">
              <label>Alinhamento Horizontal</label>
              <ul>
                <li :data-event="'flex-start'">
                  <i class="bx bx-align-left">
                    <input type="number" v-model="alignment.horizontal.x">
                  </i>
                </li>
                <li :data-event="'center'">
                  <i class="bx bx-align-middle">
                    <input type="number" v-model="alignment.horizontal.x">
                  </i>
                </li>
                <li :data-event="'flex-end'" class="active">
                  <i class="bx bx-align-right">
                    <input type="number" v-model="alignment.horizontal.x">
                  </i>
                </li>
              </ul>
            </div>
            <div class="input align" ref="align_vertical">
              <label>Alinhamento Vertical</label>
              <ul>
                <li :data-event="'flex-end'">
                  <i class="bx bx-vertical-bottom">
                    <input type="number" v-model="alignment.vertical.x">
                  </i>
                </li>
                <li :data-event="'center'">
                  <i class="bx bx-reflect-horizontal">
                    <input type="number" v-model="alignment.vertical.y">
                  </i>
                </li>
                <li :data-event="'flex-start'" class="active">
                  <i class="bx bx-vertical-top">
                    <input type="number" v-model="alignment.vertical.z">
                  </i>
                </li>
              </ul>
            </div>
            <hr />
            <div class="input">
              <span class="input-title">
                <label>Tamanho da Fonte </label>
                <small>{{ fontSize }}</small>
              </span>
              <input type="range" min="8" max="32" v-model="fontSize" />
            </div>
          </div>
        </div>
        <div class="tab-pane fade" id="actions">
          <div class="inputs cScroll">
            <div class="input">
              <label>Ação</label>
              <select v-model="action">
                <option :key="key" :value="value" v-for="({ name, value }, key) in actions">
                  {{ name }}
                </option>
              </select>
            </div>
            <div class="input" data-action="whatsapp" ref="action_phone">
              <label>Número do Whatsapp</label>
              <input type="text" placeholder="Ex: 73 99999-9999" v-mask="['(##) ####-####', '(##) #####-####']"
                v-model="wppNumber" />
            </div>
            <div class="input" data-action="whatsapp" ref="action_phone_message">
              <label>Texto da Mensagem</label>
              <input type="text" placeholder="Ex: Poderia me informar sobre o produto?" v-model="message" />
            </div>
            <div class="input" data-action="openPage" ref="action_link" data-field="input">
              <label>Link da Página</label>
              <input type="text" placeholder="Ex: site.com" v-model="link" />
            </div>
            <div class="alert" v-if="getDescription(action, actions)">
              {{ getDescription(action, actions) }}
            </div>
            <hr />
            <div class="input" data-field="select">
              <label>Filtrar Visitantes</label>
              <select v-model="only">
                <option :value="value" :key="key" v-for="({ name, value }, key) in visitors">
                  {{ name }}
                </option>
              </select>
            </div>
            <div class="alert" v-if="getDescription(only, visitors)">
              {{ getDescription(only, visitors) }}
            </div>
            <hr />
            <div class="input" ref="stock" data-field="select">
              <label>Filtrar Estoque</label>
              <select v-model="stock">
                <option :key="key" :value="value" v-for="({ name, value }, key) in []">
                  {{ name }}
                </option>
              </select>
            </div>
          </div>
        </div>
      </div>
      <div class="salvar" ref="salvar_template">Salvar Tag</div>
    </section>
    <section class="product">
      <div ref="product">
        <span :data-store="'product-item-image'">
          <img :src="defaultImage" alt="produto" />
          <div ref="tag_container" class="label">
            <span>{{ tag }}</span>
            <img :src="logo_app" alt="app" />
          </div>
        </span>
        <div class="description" :data-store="'product-item-info'">
          <p>Camiseta Preta longa</p>
          <strong>R$ 23,00</strong>
        </div>
        <div class="pay">
          <button>Comprar</button>
          <button>Espiar</button>
        </div>
      </div>
    </section>
  </div>
</template>
