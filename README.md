# nuxt-wysiwyg

import Vue from 'vue'
import $ from 'jquery'
var Wysiwyg = {
  controls: {
    undo: {
      icon: 'fa-undo'
    },
    redo: {
      icon: 'fa-repeat'
    },
    bold: {
      icon: 'fa-bold'
    },
    italic: {
      icon: 'fa-italic'
    },
    underline: {
      icon: 'fa-underline'
    },
    strikeThrough: {
      icon: 'fa-strikethrough'
    },
    justifyLeft: {
      icon: 'fa-align-left'
    },
    justifyCenter: {
      icon: 'fa-align-center'
    },
    justifyRight: {
      icon: 'fa-align-right'
    },
    justifyFull: {
      icon: 'fa-align-justify'
    },
    indent: {
      icon: 'fa-indent'
    },
    outdent: {
      icon: 'fa-outdent'
    },
    insertUnorderedList: {
      icon: 'fa-list-ul'
    },
    insertOrderedList: {
      icon: 'fa-list-ol'
    },
    h1: {
      icon: 'fa-heading'
    },
    h2: {
      icon: 'fa-heading'
    },
    h3: {
      icon: 'fa-heading'
    },
    p: {
      icon: 'fa-paragraph'
    },
    subscript: {
      icon: 'fa-subscript'
    },
    superscript: {
      icon: 'fa-superscript'
    }
  },
  init: function(content, nuxt) {
    var self = this;
    $('.wysiwyg').each(function(index, el) {
      console.log("Found WYSIWYG", el, index);
      var content = $(el).html();
      $(el).empty();

      var controls = $('<ul>')
      $.each(self.controls, function(role, options) {
        controls.append(
          $('<li>').append(
            $('<a>').data("role", role).append(
              $('<em class="fa">').addClass(options.icon)
            ).on("click", function(e) {
                e.preventDefault();

                switch ($(this).data('role')) {
                  case 'h1':
                  case 'h2':
                  case 'p':
                    //nuxt.$refs.text.select();
                    document.execCommand('formatBlock', false, $(this).data('role'));
                    break;
                  default:
                    console.log($(this).data('role'))
                    //nuxt.$refs.text.select();
        
                    console.log(nuxt)

                    nuxt.execCommand($(this).data('role'), false, null);
                    //document.execCommand($(this).data('role'), false, null);
                    break;
                }

                 $(".output", el).val($('.content', el).html());

              })
          )
        )
      })
      $(el).append(controls).append(
        $('<div class="content" contenteditable>').html(content).bind('blur keyup paste copy cut mouseup', function(e) {
          $(".output", el).val($('.content', el).html());
        })
      ).append(
        $('<input type="hidden" class="output" />')
      )
    })
  },
  set_content: function() {
    $('.wysiwyg').each(function(index, el) {
      $(".output", el).val($('.content', el).html());
    })
  }
}

if (process.BROWSER_BUILD) {
  Vue.use(Wysiwyg)
}

export default Wysiwyg
