<template>
    <Page>
        <ActionBar>
            <Label text="Home"/>
        </ActionBar>

        <GridLayout rows="auto, *">
            <Button text="download" @tap="download" />
            <ListView for="log in logs" row="1">
                <v-template>
                    <Label :text="log" />
                </v-template>
            </ListView>
        </GridLayout>
    </Page>
</template>

<script>
import * as fs from '@nativescript/core/file-system';
import { DownloadProgress } from 'nativescript-download-progress';

export default {
    data () {
        return {
            logs: []
        }
    },
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
                            this.logs.push(`Success file ${i}: ${f.name}`);
                        })
                        .catch(e => {
                            this.logs.push(`Error file ${i}: ${e.message}`);
                        })
                }
            }
            catch (e) {
                this.logs.push(`Error: ${e.message}`);
            }
            this.logs.push('Finish!');
        }
    }
};
</script>
