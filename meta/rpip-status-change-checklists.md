
# RPIP Status Change Checklists

## Draft Checklist

- [ ] Ensure that the RPIP preamble is present and correctly formatted as per RPIP-1.
  - [ ] Add RPIP number if not present.
  - [ ] Check `title` is a few words, and not a complete sentence.
  - [ ] Check `author` field matches convention.
  - [ ] Check `discussions-to` field is present, and links to the Rocket Pool forum. This field should consist of a single URL.
  - [ ] Check `status` is set to 'Draft'
  - [ ] Check that `type` is a valid value.
    - [ ] If the `type` is 'protocol', ensure a `category` is present. 
  - [ ] Check that `created` is set to the day of the RPIP number assignment, in the YYYY-MM-DD format.
- [ ] Ensure that the RPIP matches the RPIP template in terms of layout.
- [ ] Ensure that the RPIP author is content with the draft being published on the RPIPs portal in its current state.

## Finalize Checklist
- [ ] Ensure that the RPIP Draft Checklist has been completed for this RPIP.
- [ ] Ensure that the RPIP preamble is present and correctly formatted for a finalized RPIP.
  - [ ] Check `status` is set to 'Final'
- [ ] Ensure that the finalization PR is the only open pull request modifying the RPIP in the official RPIP repository.
- [ ] Editor Responsibilities
  - [ ] Check that the RPIP is ready, sound and complete.
  - [ ] Check that the ideas within the RPIP make technical sense.
  - [ ] Check that the RPIP title accurately describes the RPIP content.
  - [ ] Check the RPIP for language, markup, and code style.
- [ ] Ensure that the RPIP author has confirmed the RPIP is ready to be finalized.
- [ ] Ensure that the RPIP editors have confirmed the RPIP is ready to be finalized.

## Withdrawn Checklist
- [ ] Ensure that the RPIP author has confirmed that they are withdrawing the RPIP.
- [ ] Check `status` is set to 'Withdrawn'

## Stagnant Checklist
- [ ] Ensure that RPIP editors are in general agreement that the RPIP has been inactive for too long.
- [ ] Attempt to confirm the status change with the author if they are still a regular in the Rocket Pool community.
- [ ] Check `status` is set to 'Stagnant'

## Living Update Checklist
- [ ] Check `status` is set to 'Living'
- [ ] Add or update the `updated` date field in the format of YYYY-MM-DD. 
- [ ] Confirm any associated content changes with the other RPIP editors.

## Vote Update Checklist
- [ ] Add or update the `vote-to` field. The value should be a URL pointing to the outcome of the vote.
- [ ] Add or update the `vote-date` field. The date should be in YYYY-MM-DD format, and should match the date the vote concluded.
- [ ] Add the `vote-result` field. This should be the vote result: 'Passed'  or 'Failed'
  - [ ] This field should not be updated to 'Failed' for Living RPIPs that have previously had successful votes.





