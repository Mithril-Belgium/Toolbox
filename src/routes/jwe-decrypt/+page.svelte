<script>
import * as jose from 'jose';
import { Buffer } from 'buffer'
globalThis.Buffer = Buffer


const keys2 = {
    qual: {
        authToken : "V8IWuorLIQOk_j1e",
        stateToken: "pQWGpbmyj4En_88v"
    },
    prod: {
        authToken : "lVTGoyMtcbDGB_E9",
        stateToken: "xFxBs57xG3KEm_Vw"
    }
}

const jwe2 = "eyJhbGciOiJBMTI4S1ciLCJlbmMiOiJBMTI4Q0JDLUhTMjU2IiwidHlwIjoiSldUIn0.RcS68Cf0TkjtD99TyMY2MJHOjlx8OUBLDfflYguHrhYDHupqS5mm-A.coeR-UoGLbHugrIyCjdWzw.v6bW7WOzzWOqjSxcxRZ8ioZSX4JsUZS-CB4BvccRtXBtpGO8AkpNvgdz7ARb1hv1iu1LnxRXhdIm2m-4sPisUFuJYeIIgI2NC_HPr6TS0zb92lO8RN7QnRTt06b9exOpA7AJvNHFXJoDngDdoFgQPW2vAGGsUMueTl7AbXN3AA8GW2bQaGwJxQC_xP2sBccRJyxbtQ1RQy25VI4LzB-T5OCvKFzcJvZWurfIxiPBa4XtuaIKieGrUANB5ywjirrFURz83C654zttUuix1PFFchooMaYpqpNjUsGwAAOzILb-LkBD8hPB-SOE3wR2WZTa1FG_fERsNkDJMROaIoTKx09o_dVh6hZogxDS_U7G6xj9ThDQGGm3ixVQoBxFNMm7a2hlVvY6XG6woorG2Wbx6asOsFhqaiQBzh8YBZGXf9xz6PrF9tWr1T6S9R0F2bek0XFxKgY_TKJC3UuXKXtmsUcRBZS5P29G5OTFbjwH7Wn-PVPihu_9myidqr0tViSKdqNTWd-IVppldVzBNDr9OfD0oYq7kxfuK0wpGqk_4o68L_EV1SPUyyisM1LrG2e0I8bGwohibvYWHEKfCPsgQjgU2GhIrSmtHN5HyIl3XiKnbvmp2drHBoO_TFQWRc49OHsiZ8FQV3K0YD5q9il2B4VbCXr-JX1X874wUL8ccMEFlqCeK-V-uoQWn_dgAM_Lhll3lwb-TFe8VBW9UcRrnQ.kxOKtcV1aT2TfJcDM0IE4Q";

async function decrypt() {
    const { plaintext, protectedHeader } = await jose.compactDecrypt(jwe, Buffer.from(secret));

    const jwt = new TextDecoder().decode(plaintext)
    const bodyBase64 = jwt.split(".")[1];
    const bodyDecoded = atob(bodyBase64);
    return JSON.parse(bodyDecoded);
}

let jwe = null;
let secret = null;

let decryption = null;

function decryptClicked() {
    decryption = decrypt();
}
</script>

<div class="flex flex-row card gap-3">
    <div class="flex flex-col gap-3">
        <textarea bind:value={jwe} placeholder="Paste your JWE here" cols="100" rows="10"></textarea>
        <input bind:value={secret} placeholder="Give me your secret !" />
        <button disabled="{!jwe || !secret}" on:click={decryptClicked} class="p-5 bg-blue-500 disabled:bg-blue-300 rounded-md text-gray-50">Decrypt!</button>
    </div>
    <div class="p-5">
        {#await decryption}
	        <p>...waiting</p>
        {:then body}
            {#if body}
                
                {#each Object.entries(body) as [key, value]}
                <div>
                    <span class="inline-block w-[150px]">{key}</span>
                    {#if ["iaf", "nbf", "exp"].includes(key)}
                        <span>{value} ({new Date(value*1000).toLocaleString()})</span>
                    {:else}
                        <span>{value}</span>
                    {/if}
                    </div>
                {/each}
            {/if}
        {:catch error}
            <p class="text-red-500">{error.message}</p>
        {/await}
    </div>
</div>


<style lang="postcss">
    input, textarea {
        @apply p-5 border-2 resize-none rounded-md border-gray-300;
    }
</style>