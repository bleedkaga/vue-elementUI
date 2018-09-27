<template>
  <transition name="msgbox-fade">
    <div 
        class="el-message-box__wrapper"
        tabindex="-1"
        v-show="visible"
        @click.self="handleWrapperClick"
        role="dialog"
        aria-modal="true"
        :aria-label="title || 'dialog'">
        <div class="el-message-box" :class="[customClass, center && 'el-message-box--center']">
          <div class="el-message-box__header" v-if="title !== null">
            <div class="el-message-box__title">
              <div :class="['elmessage-box__status', icon]"
                    v-if="icon && center">
                    <span>{{title}}</span>
              </div>
            </div>

            <button 
                  type="button"
                  class="el-message-box__headerbtn"
                  aria-label="Close"
                  v-if="showClose"
                  @click="handleAction(distinguishCancelAndClose?'close':'cancel')"
                  @keydown.enter="handleAction(distinguishCancelAndClose ? 'close' : 'cancel')">
                  <i class="el-message-box__close el-icon-close"></i>
            </button>
          </div>

          <!-- content -->
          <div class="el-message-box__content">
            <div :class="['el-message-box__status', icon]"
                  v-if="icon && !center && message !== ''">
            </div>
            <div class="el-message-box__message" v-if="message !== ''">
              <slot>
                <p v-if="!dangerouslyUseHTMLString">{{message}}</p>
                <p v-else v-html="message"></p>
              </slot>
            </div>
            <div class="el-message-box__input" v-show="showInput">
              <input v-model="inputValue"
                      :type="inputType"
                      @keydown.enter.native="handleInputEnter"
                      :placeholder="inputPlaceholder"
                      ref="input">
              <div class="el-message-box__errormsg" :style="{visibility: !!editorErrorMessage ? 'visible' : 'hidden'}">
                {{editorErrorMessage}}
              </div>
            </div>
          </div>

          <!-- btns -->
          <div class="el-message-box__btns">
            <button :loading="cancelButtonLoading"
                    :class="[cancelButtonClasses]"
                    v-if="showCancelButton"
                    :round="roundButton"
                    :size="small"
                    @click="handleAction('cancel')"
                    @keydown.enter="handleAction('cancel')">
              {{cacelButtonText}}        
            </button>
            <button :loading="comfirmButtonLoading"
                    :class="[comfirmButtonClasses]"
                    v-if="showComfirmButton"
                    :round="roundButton"
                    :size="small"
                    @click="handleAction('comfirm')"
                    @keydown.enter="handleAction('comfirm')">
              {{comfirmButtonText}}        
            </button>
          </div>
        </div>

    </div>
  </transition>
</template>
