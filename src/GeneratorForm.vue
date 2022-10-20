<template>
  <div>
    <div class="row">
      <div class="col" style="margin-right: 16px;">Цвет: </div>
      <div class="col">
        <input type="color" v-model="color" value="#000000" style="width:100px">
      </div>
    </div>
    <div class="row">
      <div class="col">
        <button class="button" style="margin-right: 16px" @click="getWordsParts()">Сгенерировать</button>
        <button class="button__answers" @click="mode='answers'">Ответы</button>
      </div>
    </div>
    <div style="display: flex" v-if="words.length">
      <div class="result">
      <div class="phone-ring" :style="{'background-color': color}">
        <div class="phone-ring__sep"></div>
        <div class="phone-ring__values" v-if="set.length">
          <div
              v-for="(p, idx) in set" :key="idx"
              :class="`phone-ring__value phone-ring__value--val${idx}`">
            <div class="phone-ring__value-letters">{{ p }}</div>
            <div class="phone-ring__value-num">{{ idx > 8 ? 0: idx + 1 }}</div>
          </div>
        </div>
      </div>
    </div>
      <div class="codes">
        <template v-for="(word, idx) in words">
          <div :key="idx" class="codes__code">
            <div v-if="getCode(word) && getCode(word).length === 3">
              {{ getCode(word).join('') }} <span v-if="mode==='answers'">{{ word }}</span><span v-else>__________</span>
            </div>
          </div>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
function shuffle(array) {
  let currentIndex = array.length,  randomIndex

  while (currentIndex != 0) {
    randomIndex = Math.floor(Math.random() * currentIndex)
    currentIndex--
    [array[currentIndex], array[randomIndex]] = [
      array[randomIndex], array[currentIndex]]
  }
  return array;
}
function removeDuplicates(array) {
  return [...new Set(array)];
}
function randomInt(min, max) {
  return Math.floor(Math.random() * (max - min + 1) + min)
}
const alphabet = [
  'й',
  'ц',
  'у',
  'к',
  'е',
  'н',
  'г',
  'ш',
  'щ',
  'з',
  'х',
  'ъ',
  'ф',
  'ы',
  'в',
  'а',
  'п',
  'р',
  'о',
  'л',
  'д',
  'ж',
  'э',
  'ё',
  'я',
  'ч',
  'с',
  'м',
  'и',
  'т',
  'ь',
  'б',
  'ю'
]
const words4 = "авто ажур азот аист алоэ альт ария атом банк бант баня бард барс баул баян беда бинт бита блеф блик блин блиц блок боец бокс болт борт борщ брак брат бриг бриз брод бром брус бунт буря бусы буян быль бюро бюст бязь вдох веко вена вера верх весы вето веха вещь взор виза вилы вина винт вкус вмиг внук вода воин волк воля ворс воск вояж враг врач вред вход выпь высь вьюн вязь гать герб герц гимн гипс гиря глаз гнев гном гора горн готы граб град граф грач гриб грим гриф гром грот груз гусь гуща даль дань двор дева дело день депо дёрн дети джаз джем джин джип джут диво диез диод диск дитя дичь доля дома дочь драп друг дуга дума духи душа дуэт дыня дюйм дюны елка ежик енот жабо жало жанр жгут жезл жена жест жнец жрец жюри заря заяц звон звук зима злак змея знак зной зола зона зонд зонт зубр зыбь зять игла игра идея изба изюм икра иней итог июль июнь кадр кант карп кафе квас кедр кеды кета кило киль кино киса клад клан клей клен клин клич клуб ключ ковш кожа коза конь кора корж корм корт корь коса кофе краб край кран крем крик кров крой крот круг крюк кума купе курс куст куча лавр лань латы леди лень лето лига липа лира лиса лист литр лифт лицо ложе ложь лоза лорд лоск лось лужа луна лупа лыжи люди люкс мазь март марш матч мать маяк мгла медь межа мель меню мера метр миля мина мирт мода мозг моль мопс море морж морс мост мощь мрак муза мука мыло мышь мята небо нерв неуч нива нимб нить ниша нога ноль нора нота ночь ноша нрав нуга обед обет овал овес овца один омар омут опал опыт орда орел орех осел осот отец охра очаг очки паек пакт пари парк паук пена пень перл перо печь пика пике пила пион пирс пища план плац плащ плед плов плод плот плуг плюс плюш плющ пляж поле полк пони пора порт пост поэт пояс приз пруд прут пуля пума пуск путь пыль пюре пядь пять рагу ранг рань рапс рать раут рейд рейс река репа речь ринг риск ритм роба роды рожь роза роль ромб роса рост рота роща руда рука руль руно рыба рысь рябь сани сари сбой сбор сват свая свет свод сгиб сейф село сель семь семя сени сено сень сера серп сеть сеча сидр сила синь сито сказ скат скит скиф след слет слог слой слом слон слух смак смех снег сноп сова сода соль сонм сорт соты софа союз спад спех спец спор срез срок сруб стаж стая стих стог сток стол стоп стук стул стык табу тайм танк тело темп темя тент тень терн течь теща тигр тина тире тишь ткач тмин толк топь торг торс торф трап трек трио трое трон трос трус трюк трюм туба тура туча тушь тьма тюль тяга убор угар угол угон удав удар удел уезд ужин узда узел узор указ укол укор улей улов унты упор уран урна урок урон уста утес утка утро утюг учет ушат ушиб фаза факт фарс фарт фарш фетр филе финт флаг флот фойе фонд форт фрак фтор фунт фура хаки хаос хват хвощ хвоя хлам хлеб хлев хлор холм храм хрен хром царь цвет цель цена цикл цинк цирк чадо чары часы чека чело чета чехи чтец чудо шаль шанс шарф швец швея шейх шелк шест шило шина ширь шифр шкаф шкив шлем шлюз шлях шнур штаб шуба щека щука щепа щель эльф эпос этаж этап этил этюд эфир юань юнец юнга юмор юбка юрта явка явор ядро язык яйцо ярус ясли яхта яшма ящик";

export default {
  data() {
    return {
      mode: 'codes',
      color: '#000000',
      words4: words4.split(' '),
      words: [],
      alph: alphabet,
      allParts: [],
      wordsParts: [],
      wordsSets: {},
      wordsSets2: [],
      set: []
    }
  },
  mounted() {
    this.getAllParts()
  },
  methods: {
    getAllParts() {
      this.allParts = [...this.alph]
      for (let i = 0; i < this.alph.length; i++) {
        for (let j = 0; j < this.alph.length; j++) {
          if (this.alph[i] !== this.alph[j]) {
            this.allParts.push(`${this.alph[i]}${this.alph[j]}`)
          }
        }
      }
      this.allParts = shuffle(this.allParts)
    },
    getWordsParts() {
      this.mode = 'codes'
      this.words = [];
      [0, 1, 2, 3].forEach(() => {
        this.words.push(this.words4[randomInt(0, this.words4.length - 1)])
      })
      this.words.forEach((word) => {
        word.split('').forEach((letter) => {
          this.wordsParts.push(letter)
        })
        for (let i = 0; i < word.length - 1; i++) {
          this.wordsParts.push(`${word[i]}${word[i+1]}`)
        }
      })
      this.wordsParts = removeDuplicates(this.wordsParts)
      this.getSets()
    },
    getSets() {
      this.words.forEach((word, idx) => {
        this.wordsSets[word] = []
        this.wordsSets2[idx] = { word: word, parts: [] }
        this.wordsParts.forEach((part) => {
          if (word.includes(part)) {
            this.wordsSets[word].push(part)
          }
        })
        for (let i = 0; i < this.wordsSets[word].length; i++) {
          for (let j = 0; j < this.wordsSets[word].length; j++) {
            for (let k = 0; k < this.wordsSets[word].length; k++) {
              if (i !== j && j !== k) {
                if (`${this.wordsSets[word][i]}${this.wordsSets[word][j]}${this.wordsSets[word][k]}` === word) {
                  const arr = [this.wordsSets[word][i], this.wordsSets[word][j], this.wordsSets[word][k]]
                  this.wordsSets2[idx].parts.push(arr)
                }
              }
            }
          }
        }
      })
      this.getAllVars()
    },
    getAllVars() {
      let f = []
      const wordLength = this.wordsSets2[0].parts.length
      this.wordsSets2.forEach((word) => {
        f.push(...word.parts[randomInt(0, wordLength - 1)])
      })
      f = removeDuplicates(f)
      if (f.length > 10) {
        try {
          this.getAllVars()
        } catch(e) {
          console.log(e)
        }
      } else {
        if (f.length < 10) {
          for (let k = 0; k < 10 - f.length; k++) {
            f.push(this.allParts[randomInt(0, this.allParts.length)])
          }
        }
        this.set = shuffle(f)
        this.words.forEach((w) => {
          const code = this.getCode(w)
          if (code && code.length < 3) {
            this.getWordsParts()
          }
        })
      }
    },
    getCode(word) {
      let k = []
      this.set.forEach((s, idx) => {
        if (s.length === 2 && word.includes(s)) {
          k.push(idx)
        }
      })
      this.set.forEach((s, idx) => {
        if (s.length === 1 && word.includes(s)) {
          const eI = k.findIndex((ki) => this.set[ki].includes(s))
          if (eI) {
            k.push(idx)
          }
        }
      })
      if (k.length === 3) {
        return k.sort((a, b) => {
          const idxObA = word.indexOf(this.set[a])
          const idxOfB = word.indexOf(this.set[b])
          return idxObA - idxOfB
        }).map((i) => i > 8 ? 0 : i + 1)
      }
      else {
        this.getWordsParts()
      }
    }
  }
}
</script>
<style>
.form {
  margin: 0 auto;
  width: 500px;
  padding: 24px;
  border: 1px solid #9f9f9f;
  border-radius: 6px;
}
.row {
  width: 500px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 26px;
}
.row:last-child {
  margin-bottom: 0;
}
.col {
  width: fit-content;
}
select {
  padding: 4px 6px;
  width: 80px;
}
button {
  border: 1px solid #9f9f9f;
  padding: 6px 10px;
  border-radius: 4px;
}
.button__answers {
  margin-left: auto;
  margin-right: auto;
}
.phone-ring {
  width:500px;
  height: 500px;
  border-radius: 500px;
  position: relative;
}
.phone-ring:after {
  content: "";
  position: absolute;
  top: 135px;
  left: 135px;
  width: 230px;
  height: 230px;
  border-radius: 230px;
  background: white;
}
.phone-ring__sep {
  position: absolute;
  background: white;
  width: 100px;
  height: 40px;
  bottom: 243px;
  right: -24px;
  border-radius: 50px;
}
.phone-ring__values {
  position: absolute;
  top: 0;
  left: 0;
}
.phone-ring__value {
  width: 80px;
  height: 80px;
  border-radius: 80px;
  background: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: absolute;
}
.phone-ring__value--val0 {
  left: 369px;
  top: 120px;
}
.phone-ring__value--val1 {
  top: 46px;
  left: 290px;
}
.phone-ring__value--val2 {
  top: 30px;
  left: 184px;
}
.phone-ring__value--val3 {
  top: 74px;
  left: 87px;
}
.phone-ring__value--val4 {
  top: 169px;
  left: 32px;
}
.phone-ring__value--val5 {
  top: 278px;
  left: 42px;
}
.phone-ring__value--val6 {
  top: 362px;
  left: 107px;
}
.phone-ring__value--val7 {
  top: 395px;
  left: 210px;
}
.phone-ring__value--val8 {
  top: 361px;
  left: 314px;
}
.phone-ring__value--val9 {
  top: 277px;
  left: 380px;
}
.phone-ring__value-letters {
  text-transform: uppercase;
  font-size: 26px;
  font-weight: bold;
}
.phone-ring__value-num {}
.codes {
  margin-top: 120px;
  margin-left: 50px;
  font-size: 42px;
}
.codes__code {
  margin-bottom: 8px;
}
.codes__code:last-child {
  margin-bottom: 0;
}
</style>
