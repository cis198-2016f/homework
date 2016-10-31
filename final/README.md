# Final Project

## Schedule

* Tue. 11/08 - Proposal Presentation (in class) (5 min)
* Wed. 12/07 - Final Report (PDF via Canvas)
* Tue. 11/15 - Milestone 1 Presentation (in class) (3-5 min)
* Tue. 11/29 - Milestone 2 Presentation (in class) (3-5 min)
* Tue. 12/06 - Final Presentation (in class) (10 min)
* Wed. 12/07 - Final Report (README)

## Grading

This project is worth 40% of the final course grade.

* 15%: Proposal and Presentation
* 20%: Milestone 1 Presentation and Status
* 20%: Milestone 2 Presentation and Status
* 45%: Final Presentation, Status, and Report

**Important:** "Status" grading will be based primarily on your documentation.
Spend time on this and write in-depth documentation!
You'll need to use rustdoc to generate documentation before each presentation
(see below: [Documentation](#documentation)).

## Project Guidelines

* Groups may be 1-2 people.

* You can either make your own project or contribute to an existing open-source
  project. If you do the latter, you should speak to the project maintainers
  first, and make sure you're able to finish your project :)

* If you start a new project, we would love to see stuff that hasn't been done in
  Rust before!

* We expect you to do about (3 weeks * 0.5 credit) worth of work per person -
  but what that means is subjective. More work will mean a more impressive
  project - but it's much more important that you finish!

* If your idea is in danger of being too big, you need to have a plan for how
  to scale it back. Carve out a reasonable subset of the idea as your baseline,
  and have additional goals on top of that.

* We would really like your project to be open-source!
  You don't need to pick licensing immediately, but consider Apache and MIT.

### Ideas / Libraries

* Piston (GUI toolkit)
* nix (libc wrapper)
* Rewriting slow parts of other programs (e.g. Python/Ruby)
* Distributed system / networking
    * Implementing a communication protocol stack or library
    * Distributed server
* Write a server for a particular protocol (ssh, ntp, ftp, http)
* Some kind of game

## Proposal Document

* Your proposal document should be about 1 page. Submit as a PDF on Canvas.

* List your group members.

* Abstract: Your idea in a sentence or two.

* Open-source or not. If not, why?
  You don't need to pick a license right now, but Apache and MIT are good.

* Project outline: Your minimal goals, expected goals, and stretch goals.
    * This should be detailed enough that we can judge the complexity and merit.
    * Include appropriate technical details of each goal.

* Tentative Schedule: Which goals do you want to finish each week? Milestone
  presentations will be at the beginning of class each week.
    * You don't need to stick strictly to this schedule, but it's good to have a

## Milestone Presentations

These will be very quick (3-5 min). Update us on your progress: have you learned
anything interesting? Completed a major part of your project? Come across an
unexpected problem?

All of your work must be in your repository by class time, including
rustdoc documentation for the work you're presenting (see below).
If you're contributing, this means it should be in your fork.

## Documentation

If you are working on a standalone project, you should have rustdocs documenting
all of the work you've done so far, at each milestone and the final
presentation. This doesn't mean documenting every single function - but you need
detailed documentation for the modules, functionality, and all important
structs/functions.

**Important:** Spend time on this and write in-depth documentation.
It will be a significant part of our grading!

For any general information, put it on your crate root module. You can use
this syntax for module documentation:

```
//! if you put this documentation comment style in a
//! module, it will apply to the module itself instead
//! of the thing after it
```

Please host docs somewhere online. You can use
[hosting on Eniac](http://www.seas.upenn.edu/cets/answers/webpage.html),
or you may use a [GitHub Pages](https://pages.github.com/) project site.

By default, `cargo doc --no-deps` will export documentation for everything
_public_ in your crate. However, for the project, you'll likely want to
export everything. To do this, use:

```sh
cargo rustdoc -- --no-defaults --passes collapse-docs --passes unindent-comments
```

Here is some [example rustdoc output](http://cis198-2016s.github.io/final-sample-rustdoc/webchat/).
This shows the output of the above command on our HW07 solution. Note that it
doesn't have any _module-level_ documentation - which will be most important
for you.

If you're contributing to another project, you should send us a compilation of
the documentation for everything **you** have written as part of your project.

If there is any additional extra information that you want to send to us but
think doesn't belong in the documentation, email it to us before your
presentation.

## Final Report

Write your report in markdown, and save it as `REPORT.md` in your repository
root, and we'll find it there. If you don't have a dedicated repository for this
project (e.g. you are forking another project) or you plan to immediately
publicize your project, you can submit via Canvas. If you have screenshots or
images, save them in the repo and embed them in `REPORT.md`.

We'll expect about 2-3 printed-pages-worth of info. Include whatever is
appropriate or important for your project, such as:

* Summary
* Accomplishments
* Components, structure, design decisions
* Testing approach and results
* Benchmarks
* Limitations
* Postmortem
    * What went well
    * What you would do differently
* etc.

Keep in mind that if your repo is public, your report will be as well.
If you have any private feedback, email us.
