# File Upload with Validations
Client side validation with input file using jQuery

## Bootstrap
* [CSS](https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css)

* [JS](https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js)

## jQuery

* [jQuery](https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js)

## Final Output

![alt text](https://github.com/AniruddhPurohit/fileUpload-jQuery/bob/master/screen.png "Final Output")

## HTML Form

```html
<form id="myfrm" name="myfrm">
    <div class="form-group">
        <div class="row">
            <div class="col-md-2">
                <label for="userfile" class="btn btn-sm btn-primary">Select Logo</label>
                <input type="file" name="userfile" id="userfile" style="display: none;">		
            </div>
            <div class="col-md-2">
                <div id="image-preview" class="img-wrap" style="display:none;">
                    <span class="close" id="clearlogo">&times;</span>
                    <img id="preview" class="img-thumbnail" src="">
                </div>
            </div>
            <div class="col-md-8"></div>
        </div>
    </div>
</form>
```

## Custom CSS

```css
/* main div */
.img-wrap {
    position: relative;
    width: 150px;
}

/* close button to remove image */
.img-wrap .close {
    color: white;
    font-size: 29px;
    padding: 0 7px;
    border: 2px solid;
    border-color: white;
    background-color: black;
    border-radius: 50%;
    position: absolute;
    top: -15px;
    right: -15px;
    z-index: 100;
}

/* display image */
#preview {
  display: block;
  width: 100%;
  height: auto;
}
```
## jQuery

```javascript
/*
* On change of image selection
*/
$("#userfile").change(function() {
    // get HTML element
    var file_upload = event.target;

    // check if not empty
    if(file_upload.files.length > 0) {
        // get extension of selected image file
        var ext = $(this).val().split('.').pop().toLowerCase();
        
        // get the size in bytes
        var size = file_upload.files[0].size;

        // check for type of logo file
        if(ext !== 'png') {
            alert('Invalid logo file (only .png is allowed).');
            return false;
        }
        // check for size of logo file
        if(size > 2097152) {
            alert('Maximum 2MB logo size allowed.');
            return false;
        }
        // read file
        var reader = new FileReader();
        reader.onload = function (e) {
            $('#preview').attr('src', e.target.result);
        };
        reader.readAsDataURL(file_upload.files[0]);

        // display image preview
        $('#image-preview').css('display', 'block');
    } else {
        // hide image preview
        $('#image-preview').css('display', 'none');
    }
});

/*
* To clear fileinput
*/
$('#clearlogo').click(function(event) {
    reset_file();
    $('#preview').attr('src', '');
    $('#image-preview').css('display', 'none');
    return false;
});

/*
* To reset file upload control with id=file
*/
function reset_file() {
    var $file_element = $('#userfile');
    $file_element.wrap('<form>').closest('form').get(0).reset();
    $file_element.unwrap();
}
```