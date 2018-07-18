var holder = document.getElementById('holder'),
    state = document.getElementById('status');

if (typeof window.FileReader === 'undefined') {
  state.className = 'fail';
} else {
  state.className = 'success';
  state.innerHTML = 'File API & FileReader available';
}

holder.ondragover = function () { this.className = 'hover'; return false; };
holder.ondragend = function () { this.className = ''; return false; };
holder.ondrop = function (e) {
  this.className = '';
  e.preventDefault();

  var file = e.dataTransfer.files[0],
      reader = new FileReader();
  reader.onload = function (event) {
    console.log(event.target);
    holder.style.background = 'url(' + event.target.result + ') no-repeat center';
  };
  console.log(file);
  reader.readAsDataURL(file);

  return false;
};

edo.directive('fileDrag', function () {
  return {
    restrict: 'A',
    link: function (scope, elem) {
      elem.bind('drop', function(e){
        e.preventDefault();
        e..stopPropagation();
        var file = e.dataTransfer.files[0], reader = new FileReader();
          reader.onload = function (event) {
          console.log(event.target);
          elem.style.background = 'url(' + event.target.result + ') no-repeat center';
        };
        console.log(file);
        reader.readAsDataURL(file);

        return false;
      });
    }
  };
});

function fileDropzoneDirective() {
    return {
        restrict: 'A',
        link: fileDropzoneLink
    };
    function fileDropzoneLink($scope, element, attrs) {
        element.bind('dragover', processDragOverOrEnter);
        element.bind('dragenter', processDragOverOrEnter);
        element.bind('dragend', endDragOver);
        element.bind('dragleave', endDragOver);
        element.bind('drop', dropHandler);

        function dropHandler(angularEvent) {

            var event = angularEvent.originalEvent || angularEvent;
            var file = event.dataTransfer.files[0];
            event.preventDefault();
            $scope.$eval(attrs.onFileDrop)(file);

        }
        function processDragOverOrEnter(angularEvent) {
            var event = angularEvent.originalEvent || angularEvent;
            if (event) {
                event.preventDefault();
            }
            event.dataTransfer.effectAllowed = 'copy';
            element.addClass('dragging');
            return false;
        }

        function endDragOver() {
            element.removeClass('dragging');
        }
    }
}

