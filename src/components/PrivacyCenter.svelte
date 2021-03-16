<script lang="typescript">
    import {
        Alert,
        Icon,
        Dialog,
        Card,
        ExpansionPanels, ExpansionPanel,
        CardActions,
        CardTitle,
        CardText,
        Button,
        Row,
        Col,
    } from "svelte-materialify";
    import { scale } from "svelte/transition";
    import { mdiAlert } from "@mdi/js";
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
    import Checkbox from "./third-party/svelte-checkbox/Checkbox.svelte";

    let privacyDialogActive = false;
    function togglePrivacyDialog() {
        privacyDialogActive = !privacyDialogActive;
    }
    let privacyAlertActive = false;
    function togglePrivacyAlert() {
        privacyAlertActive = !privacyAlertActive;
    }
    if (
        !document.cookie.split("; ").find((row) => row.startsWith("hasAgreed"))
    ) {
        togglePrivacyAlert();
    }
    let agreeWithAll = false;
    function agreed(e) {
        if (e) {
            document.cookie =
                "hasAgreed; SameSite=Lax;expires=Fri, 31 Dec 9999 23:59:59 GMT";
            togglePrivacyAlert();
        }
    }
    let htmlContent = "";
    let policyDialog = false;
    function openPolicy() {
        policyDialog = !policyDialog;
        if (!htmlContent.length)
            fetch("yandex.html")
                .then((response) => {
                    return response.text();
                })
                .then((html) => {
                    htmlContent = html;
                });
    }
    openPolicy();
</script>

<div id="self">
    <Alert
        class="blue white-text"
        transition={scale}
        transitionOpts={{ duration: 500 }}
        bind:visible={privacyAlertActive}
    >
        <div slot="icon">
            <Icon path={mdiAlert} />
        </div>
        Как и другие сайты, мы используем файлы cookie. Вы соглашаетесь на сохранение
        всех категорий cookie на вашем устройстве и обработку персональных данных
        в порядке и целях, указанных по<a
            class="white-text"
            href="#"
            on:click={openPolicy}>ссылке</a
        >. Вы можете отказаться от некоторых категорий файлов cookie,
        пожалуйста, нажмите
        <a class="white-text" href="#" on:click={togglePrivacyDialog}
            >настроить</a
        >
        <br /><Checkbox
            on:change={agreed}
            id="main__checkbox"
            labelId="checkboxLabel"
            name="checkbox"
            size="1.7rem"
            required
            color="red"
            bind:checked={agreeWithAll}
        />
        <label
            style="cursor:pointer;text-decoration:underline"
            for="checkboxLabel">Согласен</label
        >
    </Alert>
</div>

<Dialog fullscreen persistent bind:active={privacyDialogActive}>
    <Card>
        <CardTitle>
            Я разрешаю обработку своих персональных данных
        </CardTitle>
        <CardText>
        <ExpansionPanels>
            <ExpansionPanel>
            <span slot="header" on:click|stopPropagation>
                  <Checkbox
            class="main__checkbox"
            size="1.7em"
            name="inputName"
            secondaryColor="rgba(1, 50, 67,0.2)"
            primaryColor="rgb(77, 5, 232)"
          />
                  Яндекс.Метрика</span>
                  {@html htmlContent}
            </ExpansionPanel>
            <ExpansionPanel>
                <span slot="header" on:click|stopPropagation>
                      <Checkbox
                class="main__checkbox"
                size="1.7em"
                name="inputName"
                required
                secondaryColor="rgba(1, 50, 67,0.2)"
                primaryColor="rgb(77, 5, 232)"
              />
              Защита __Host-X-CSRF-TOKEN * </span>
              Уникальный анонимный идентификатор запроса, непривязанный к пользователю
              Необходимый при использовании сайта   
              Предотвращает межсайтовую подделку запроса<br/>
              Место хранения: файл cookie и HTTP заголовки, необходимость хранения на сервере отсутсвует ввиду дублирования в HTTP заголовке
              
                </ExpansionPanel>
                </ExpansionPanels>
            </CardText>
            <CardActions>
            <Button block text class="primary-color ml-auto" on:click={togglePrivacyDialog}
            >ЗАКРЫТЬ</Button >
        </CardActions>
    </Card>
</Dialog>

<!--Dialog
    
    persistent
    bind:active={privacyDialogActive}
>
        <Card>
                    <CardTitle>YCLID</CardTitle>
                    <CardText>
                        <p>
                            <b>Цель идентификатора:</b> Отслеживание конверсий Яндекс.Директ
                        </p>
                        <p>
                            <b>Данные отправляются:</b> в Российскую Федерацию, Европу,
                            США
                        </p>
                    </CardText>
                    <CardActions>
                        <Checkbox
                            id="main__checkbox"
                            labelId="yclid_checkbox"
                            name="checkbox"
                            size="1.7rem"
                            required
                            color="red"
                        />
                        <label
                            style="cursor:pointer;text-decoration:underline"
                            for="yclid_checkbox">Согласен</label
                        >
                    </CardActions>

        </Card>
        <Card>

                    <CardTitle>GCLID</CardTitle>
                    <CardText>
                        <p>
                            <b>Цель идентификатора:</b> Отслеживание конверсий Google
                            AdWords
                        </p>
                        <p><b>Данные отправляются:</b> в Российскую Федерацию, Европу,
                            США</p>
                    </CardText>
                    <CardActions>
                        <Checkbox
                            id="main__checkbox"
                            labelId="gclid_checkbox"
                            name="checkbox"
                            size="1.7rem"
                            required
                            color="red"
                        />
                        <label
                            style="cursor:pointer;text-decoration:underline"
                            for="gclid_checkbox">Согласен</label
                        >
                    </CardActions>
        </Card>
        <Card>

               
                    <CardTitle>Яндекс Метрика</CardTitle>
                    <CardText>
                        <p>
                            <b>Цель: </b> Анализ использования сайта, отслеживание конверсий,
                                <a
                                href="#"
                                on:click={openPolicy}>подробнее</a>
                        </p>
                        <p><b>Данные отправляются:</b> в Россискую Федерацию, в Европу или в США</p>
                    </CardText>
                    <CardActions>
                        <Checkbox
                            id="main__checkbox"
                            labelId="gclid_checkbox"
                            name="checkbox"
                            size="1.7rem"
                            required
                            color="red"
                        />
                        <label
                            style="cursor:pointer;text-decoration:underline"
                            for="gclid_checkbox">Согласен</label
                        >
                        <Button text class="ml-auto" on:click={togglePrivacyDialog}
            >ЗАКРЫТЬ</Button
        >
                    </CardActions>
        </Card>
</Dialog>


<Dialog fullscreen={true} persistent bind:active={policyDialog}>
    <Card raised loading={!htmlContent.length}>
        <div>
            <CardText>
                {@html htmlContent}
            </CardText>
        </div>
        <CardActions>
            <Button style="{(!!htmlContent.length)? '': 'visibility: hidden;'}"
                block
                class="primary-color"
                on:click={() => (policyDialog = !policyDialog)}>ЗАКРЫТЬ</Button
            >
        </CardActions>
    </Card>
</Dialog-->

<style lang="scss">
    $chk-margin: 0.5em;
    :global(.main__checkbox) { 
        display: inline-block;
        margin: $chk-margin $chk-margin 0 0;
        vertical-align: -.5em;
    }
    /*:global(#main__checkbox) {
        display: inline-block;
        margin: $chk-margin $chk-margin 0 0;
        vertical-align: -$chk-margin;
        --checkbox-color-secondary: #ddd !important;
        --checkbox-color-primary: #fff !important;
    }*/
    /*:global( > #main__checkbox) {
        margin: 0 $chk-margin 0 0;
        --checkbox-color-secondary: #aaa !important;
        --checkbox-color-primary: #000 !important;
    }*/
    #self {
        position: fixed;
        bottom: -1em;
    }
</style>
