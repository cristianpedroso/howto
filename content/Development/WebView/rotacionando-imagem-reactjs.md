Quando recebemos uma iamgem do nativo, e a foto foi tirada pela camera, encontramos alguns problemas em virtude da rotacao retornada ao JS, mas para resolver esse problema, é susse!
1. Baixa a biblioteca exif-js: `npm install --save exif-js`
2. Importe a biblioteca baixada: `import {EXIF} from 'exif-js';`

3. Crie no seu HTML um canvas, que sera aonde a imagem sera transformada e rotacionada
```
<canvas ref={c => this.canvas = c} style={{visibility: 'hidden', position: 'absolute', width:0, height:0}}/>
```
4. Crie a funcao para rotacionar a imagem
```
// Passando a imagem como parametro
handleRotateImage(file) {
        const reader = new FileReader(); // Cria um FileReader
        const _canvas = this.canvas;
        
        // Cria a funcao para rotacionar a imagem
        reader.addEventListener('load', () => {
            const exif = EXIF.readFromBinaryFile(this.base64ToArrayBuffer(reader.result));
            let fileToSend;

            if (exif) {

                let image = new Image();
                image.src = reader.result;

                image.addEventListener('load', () => {

                    let thumb = {width: 500, height: 500};
                    let proportion = 1;
                    if (exif.PixelYDimension > exif.PixelXDimension) {
                        proportion = Math.min(exif.PixelXDimension, thumb.width) / exif.PixelYDimension;
                        thumb.width = exif.PixelXDimension * proportion;
                        thumb.height = exif.PixelYDimension * proportion;
                    } else {
                        proportion = Math.min(exif.PixelYDimension, thumb.height) / exif.PixelXDimension;
                        thumb.width = exif.PixelXDimension * proportion;
                        thumb.height = exif.PixelYDimension * proportion;
                    }

                    // set proper canvas dimensions before transform & export
                    if ([5, 6, 7, 8].indexOf(exif.Orientation) > -1) {
                        _canvas.width = thumb.height;
                        _canvas.height = thumb.width;
                    } else {
                        _canvas.width = thumb.width;
                        _canvas.height = thumb.height;
                    }

                    const ctx = _canvas.getContext("2d");

                    // transform context before drawing image
                    switch (exif.Orientation) {
                        case 2:
                            ctx.transform(-1, 0, 0, 1, thumb.width, 0);
                            break;
                        case 3:
                            ctx.transform(-1, 0, 0, -1, thumb.width, thumb.height);
                            break;
                        case 4:
                            ctx.transform(1, 0, 0, -1, 0, thumb.height);
                            break;
                        case 5:
                            ctx.transform(0, 1, 1, 0, 0, 0);
                            break;
                        case 6:
                            ctx.transform(0, 1, -1, 0, thumb.height, 0);
                            break;
                        case 7:
                            ctx.transform(0, -1, -1, 0, thumb.height, thumb.width);
                            break;
                        case 8:
                            ctx.transform(0, -1, 1, 0, 0, thumb.width);
                            break;
                        default:
                            ctx.transform(1, 0, 0, 1, 0, 0);
                    }

                    ctx.drawImage(image, 0, 0, thumb.width, thumb.height);

                    // Seta o arquivo a imagem a ser retornada
                    fileToSend = _canvas.toDataURL("image/jpeg", 90);
                    // Chama a funcao de retorno
                    this.handleUploadImage(fileToSend);

                });
            } else {
            
                // Seta o arquivo a imagem a ser retornada
                fileToSend = reader.result;
                // Chama a funcao de retorno
                this.handleUploadImage(fileToSend);
                
            }

        });

        // Le a imagem, iniciando a funcao criada acima.
        reader.readAsDataURL(file);

}

// Funcao auxiliar
```
base64ToArrayBuffer(base64) {
        base64 = base64.replace(/^data:([^;]+);base64,/gmi, '');
        const binaryString = atob(base64);
        const len = binaryString.length;
        const bytes = new Uint8Array(len);
        for (let i = 0; i < len; i++) {
            bytes[i] = binaryString.charCodeAt(i);
        }
        return bytes.buffer;
    }
```
5. Criar a funcao de retorno após o redimensionamento da imagem
```
handleUploadImage(image) {
    // Aqui a imagem ja está em Base64 pronta para ser enviada ao back
}
```