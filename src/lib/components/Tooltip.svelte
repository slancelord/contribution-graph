<div class="tooltip">
    <span class="tooltip__header"> 
        <slot name="header"></slot>
    </span>

    {#if $$slots.footer}
      <span class="tooltip__footer">
        <slot name="footer"></slot>
      </span>
    {/if}
</div>

<style> 
    .tooltip {
        display: flex;
        flex-direction: column;
        gap: 4px;
        padding: 5px 5px;
        position: absolute;
        white-space: nowrap;
        z-index: 10;
        bottom: 100%;
        left: 50%;
        margin-bottom: 6px;
        transform: translateX(-50%); 
        background-color: #000000;
        border-radius: 4px;
    }

    .tooltip::after {
        content: "";
        position: absolute;
        top: 100%;
        left: 50%;
        transform: translateX(-50%);
        border-width: 6px 4.5px 0 4.5px;
        border-style: solid;
        border-color: #000000 transparent transparent transparent;
    }

    .tooltip__header, .tooltip__footer {
        font-family: Inter;
        font-weight: 400;
        font-style: Regular;
        line-height: 100%;
        letter-spacing: 0%;
    }

    .tooltip__header {
        font-size: 12px;
        color: #ffffff;
    }

    .tooltip__footer {
        font-size: 10px;
        color: #7c7c7c;
    }
</style>