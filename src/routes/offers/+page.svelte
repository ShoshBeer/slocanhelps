<script lang="typescript">
	import { page } from '$app/stores';
  import { TabGroup, TabAnchor } from '@skeletonlabs/skeleton';
  import Icon from '@iconify/svelte';

	export let data;
  let [type, community] = $page.url.hash.replace('#', '').split('&');

  let filteredData = data.data;

  const typeMap = {
    'Accommodation': 'fluent-emoji-high-contrast:house-with-garden',
    'Animals': 'fluent-emoji-high-contrast:goat',
    'Transportation': 'fluent-emoji-high-contrast:sport-utility-vehicle',
    'Labour': 'fluent-emoji-high-contrast:flexed-biceps',
    'Community Support': 'fluent-emoji-high-contrast:people-hugging',
    'Food': 'fluent-emoji-high-contrast:fork-knife-plate',
  };

  $: [type, community] = $page.url.hash.replace('#', '').split('&');

  $: {

    filteredData = data.data.filter(offer => {
      if ((type === 'allTypes' || !type) && (community === 'allCommunities' || !community)) return true;
      if (type === 'allTypes' || !type) {
        return offer.gristHelper_Display2 === community;
      }
      if (community === 'allCommunities' || !community) {
        return offer.Type?.some(offerType => offerType.toLowerCase().includes(type));
      }
      return offer.Type?.some(offerType => offerType.toLowerCase().includes(type)) && offer.gristHelper_Display2 === community;
    });

  };

</script>

<div>

  <h1 class="h1">Offers of Help</h1>
  <div class="mb-4 text-sm">
    If you need to change or remove your offer, please contact us 
    by <a href="mailto:romeda@gmail.com" class="anchor">email</a>, 
    text at 250-278-7978, 
    or on <a href="https://www.facebook.com/romeda" class="anchor">Facebook</a> 
    and we'll get back to you as soon as we can.
  </div>


  <hr class="!border-t-1 my-4" />

  <TabGroup
    active="variant-filled-primary"
    hover="hover:variant-soft-primary"
    class=""
    rounded=""
    border="">

    <TabAnchor href="/offers#allTypes&" selected={type === 'allTypes'}>All Offers</TabAnchor>
    <TabAnchor href="/offers#accommodation&" selected={type === 'accommodation'}>
      <div class="flex items-center">
        <Icon icon="fluent-emoji-high-contrast:house-with-garden" />&nbsp; Accommodation
      </div>
    </TabAnchor>
    <TabAnchor href="/offers#animals&" selected={type === 'animals'}>
      <div class="flex items-center">
        <Icon icon="fluent-emoji-high-contrast:goat" />&nbsp; Animals
      </div>
    </TabAnchor>
      <!-- <FluentEmojiHighContrastGoat /> Animals</TabAnchor> -->
    <TabAnchor href="/offers#transportation&" selected={type === 'transportation'}>
      <div class="flex items-center">
        <Icon icon="fluent-emoji-high-contrast:sport-utility-vehicle" />&nbsp; Transportation
      </div>
    </TabAnchor>
    <TabAnchor href="/offers#labour&" selected={type === 'labour'}>
      <div class="flex items-center">
        <Icon icon="fluent-emoji-high-contrast:flexed-biceps" />&nbsp; Labour
      </div>
    </TabAnchor>
    <TabAnchor href="/offers#support&" selected={type === 'support'}>
      <div class="flex items-center">
        <Icon icon="fluent-emoji-high-contrast:people-hugging" />&nbsp; Community Supports
      </div>
    </TabAnchor>
  </TabGroup>

  <div class="table-container w-full mt-6">
		<table class="flex w-full table-fixed flex-row flex-nowrap overflow-hidden">
			<thead>
				<tr class="border-b-1 invisible absolute mb-2 flex flex-col flex-nowrap bg-[hsl(34,50%,80%)] dark:bg-slate-950 sm:visible sm:static sm:mb-0 sm:table-row sm:rounded-none">
					<th class="pl-3 py-4 text-left text-xl font-bold md:w-48">Name</th>
					<!-- <th class="pl-1 py-4 text-left text-xl font-bold md:w-32">Community</th> -->
					<th class="pl-1 py-4 text-left text-xl font-bold md:w-1/2">Offer</th>
					<th class="pl-1 py-4 text-left text-xl font-bold md:w-48">Contact Details</th>
				</tr>
			</thead>
			<tbody class="flex-1 sm:flex-none">

				{#each filteredData as offer}
				
          <tr class="
            flex flex-col flex-nowrap align-top 
            mb-4
            pl-1 pt-2 pb-4
            rounded-xl
            sm:rounded-none
            sm:table-row
            odd:bg-orange-50 even:bg-orange-100 
            dark:text-slate-300 dark:odd:bg-slate-800 dark:even:bg-slate-900
            ">
						
            <td class="px-2 py-2" data-th="Name">
              <div class="text-xl font-medium">{offer.Name}</div>
              @ {offer.gristHelper_Display2 || offer.Community_if_Other_ || 'Other'}
            </td>
						
            <!-- <td class="px-2 py-2 sm:w-40" data-th="Community"></td> -->
						
            <td class="px-2 py-2 overflow-auto md:w-1/2" data-th="Offer">
              
              <div class="flex justify-left gap-2">
                {#if offer.Type}
                  {#each offer.Type as type}
                    <Icon icon={typeMap[type]} inline={true} class="w-8 h-8 text-slate-900 dark:text-slate-400" />
                  {/each}
                  &nbsp;
                {/if}
              </div>

              <div class="text-lg">{offer.Offer}</div>
            </td>
						
            <td class="label-it px-2 py-2 overflow-auto md:w-48 break-words align-bottom" data-th="Contact">
              <div class="text-lg">{offer.Contact_Details}</div>
            </td>
					
          </tr>
				{/each}

			</tbody>
		</table></div>

  </div>

<style>
   @media (min-width: 640px) {
    table {
      display: inline-table !important;
    }

    thead tr:not(:first-child) {
      display: none;
    }

    td:not(:last-child) {
      border-bottom: 0;
    }
  }

  @media (max-width: 640px) {
    td.label-it::before {
      content: attr(data-th)": ";
      font-size: .9rem;
      color: rgb(95, 95, 95);
      display:block;
    }
    
  }
</style>
