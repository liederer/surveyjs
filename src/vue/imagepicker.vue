<template>
    <fieldset :class="question.cssClasses.root">
        <div v-for="(item, index) in question.visibleChoices" :key="item.value" :class="getItemClass(item)" >
            <label :class="question.cssClasses.label">
                <input v-if="question.multiSelect" style="display: none;" type="checkbox" :name="question.name + '_' + question.id" :value="item.value" :id="question.inputId + '_' + item.value" v-model="question.value" :disabled="question.isReadOnly" v-bind:aria-label="question.locTitle.renderedHtml" :class="question.cssClasses.itemControl"/>
                <input v-else style="display: none;" type="radio" :name="question.name + '_' + question.id" :value="item.value" :id="question.inputId + '_' + item.value" v-model="question.value" :disabled="question.isReadOnly" v-bind:aria-label="question.locTitle.renderedHtml" :class="question.cssClasses.itemControl"/>
                <div>
                    <img :class="question.cssClasses.image" :src="item.imageLink"/>
                    <span v-if="question.showLabel" :title="item.text || item.value" :class="question.cssClasses.itemText">{{item.text || item.value}}</span>
                </div>
            </label>
        </div>
        <legend style="display: none;">{{question.locTitle.renderedHtml}}</legend>
    </fieldset>
</template>

<script lang="ts">
import Vue from "vue";
import { Component } from "vue-property-decorator";
import { default as QuestionVue } from "./question";
import { QuestionImagePickerModel } from "../question_imagepicker";

@Component
export class ImagePicker extends QuestionVue<QuestionImagePickerModel> {
  // TODO may be need to move to the model
  get colWidth() {
    var colCount = this.question.colCount;
    return colCount > 0 ? 100 / colCount + "%" : "";
  }
  getItemClass(item) {
    var itemClass =
      this.question.cssClasses.item +
      (this.question.colCount === 0
        ? " sv_q_imagepicker_inline"
        : " sv-q-col-" + this.question.colCount);
    if (this.question.multiSelect) {
      if (this.question.value.indexOf(item.value) !== -1) {
        itemClass += " checked";
      }
    } else {
        if (item.value === this.question.value) { 
            itemClass += " checked";
        }
    }
    return itemClass;
  }
}
Vue.component("survey-imagepicker", ImagePicker);
export default ImagePicker;
</script>
