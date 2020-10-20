<template>
    <Page>
        <ActionBar>
            <Label text="Home"/>
        </ActionBar>

        <GridLayout>
            <Button text="download" @tap="download" />
        </GridLayout>
    </Page>
</template>

<script>
import * as fs from '@nativescript/core/file-system';
import { DownloadProgress } from 'nativescript-download-progress';

export default {
    methods: {
        async download () {
            try {
                let folder = fs.knownFolders.documents().getFolder('test'),
                    fileUrls = new Array(10).fill('https://miro.medium.com/max/1200/1*mk1-6aYaf_Bes1E3Imhc0A.jpeg');
                await folder.clear();
                for (let i in fileUrls) {
                    const download = new DownloadProgress();
                    download.addProgressCallback(progress => {
                        console.log(`Progress file ${i}:`, progress);
                    })
                    await download.downloadFile(
                        fileUrls[i],
                        fs.path.join(folder.path, i + '.jpeg')
                    )
                        .then(f => {
                            console.log(`Success file ${i}:`, f);
                        })
                        .catch(e => {
                            console.log(`Error file ${i}:`, e);
                        })
                }
            }
            catch (e) {
                console.error(e);
            }
            console.log('Finish!');
        }
    }
};
</script>
