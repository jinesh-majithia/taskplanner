<form bind:this={form_el} class="add-task">
    <ThemedTextArea required={true} bind:value={description_value} placeholder={"Description"}/>
    <div class="btn-wrapper flex-div dir-row-reverse">
        <ThemedButton type={"submit"} --bgcolor="var(--primary)" --color="var(--foreground)" callback={primaryCallback}>Update</ThemedButton>
        <ThemedButton --bgcolor="none" --color="var(--primary)" callback={secondaryCallback}>Cancel</ThemedButton>
    </div>
</form>

<script lang="ts">
import ThemedButton from "./ThemedButton.svelte";
import ThemedTextArea from "./ThemedTextArea.svelte";
import { API_ENDPOINT } from "../store/store";
import { get } from "svelte/store";

export let secondaryCallback: VoidFunction;
export let update_data: VoidFunction;

export let description_value:string;
export let nid_value:string;

let form_el: HTMLFormElement;

const primaryCallback = async () => {
    if(!form_el.checkValidity()) return
    
    let resp = await fetch(`${get(API_ENDPOINT)}/note/change`, {
        method: 'POST',
        headers: {'Content-Type' : 'application/json'},
        credentials : 'include',
        body: JSON.stringify({
            nid:nid_value,
            content:description_value,
        })
    })
    if (resp.ok){
        secondaryCallback()
        update_data()
    }
}

</script>

<style>
.add-task{
    max-width: 450px;
    border-radius: 16px;
    background-color: #fff;
    padding: 16px;
}
.btn-wrapper{
    margin-top: 16px;
}
</style>