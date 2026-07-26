---
title: "1.2.2. The Debian Free Software Guidelines"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.foundation-documents.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.foundation-documents.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "1.2.2. The Debian Free Software Guidelines"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](the-debian-project.en.md)The Debian Administrator's Handbook[Next](sect.debian-internals.en.md)

## 1.2.Â The Foundation Documents

			A few years after its initial launch, Debian formalized the principles that it should follow as a free software project. This deliberately activist decision allows orderly and peaceful growth by ensuring that all members progress in the same direction. To become a Debian developer, any candidate must confirm and prove their support and adherence to the principles established in the project's Foundation Documents.
		
			The development process is constantly debated, but these Foundation Documents are widely supported and consensual, and thus rarely change. The Debian constitution also offers other guarantees for their stability: a three-quarter qualified majority is required to approve any amendment.
		

### 1.2.1.Â The Commitment towards Users

				The project also has a âsocial contractâ. What place does such a text have in a project only intended for the development of an operating system? It is quite simple: Debian works for its users, and thus, by extension, for society. This contract summarizes the commitments that the project undertakes. Let us study them in greater detail:
			
						Debian will remain 100% free.
					
						This is Rule No. 1. Debian is and will remain composed entirely and exclusively of free software. Additionally, all software development within the Debian project, itself, will be free.
					

PERSPECTIVE Beyond software

						The first version of the Debian Social Contract said âDebian Will Remain 100% Free Softwareâ. The disappearance of this last word (with the ratification of Version 1.1 of the contract in April of 2004) indicates the will to achieve freedom, not only in software, but also in the documentation and any other element that Debian wishes to provide within its operating system.
					
						This change, which was only intended as editorial, has, in reality, had numerous consequences, especially with the removal of some problematic documentation. Furthermore, the increasing use of firmware in drivers poses problems: many are non-free, yet they are necessary for proper operation of the corresponding hardware.
					
						We will give back to the free software community.
					
						Any improvement contributed by the Debian project to a work integrated in the distribution is sent back to the author of the work (called âupstreamâ). In general, Debian will cooperate with the community rather than work in isolation.
					

COMMUNITY Upstream author, or Debian developer?

						The term âupstream authorâ means the author(s)/developer(s) of a work, those who write and develop it. On the other hand, a âDebian developerâ usually uses an existing work to make it into a Debian package - the term âDebian maintainerâ is better suited here.
					
						In practice, there can be overlaps between both roles: the Debian maintainer may write a patch, which benefits all users of the work. In general, Debian encourages those in charge of a package in Debian to get involved in âupstreamâ development as well (they become, then, contributors, without being confined to the role of simple users of a program). Or the Debian developer creates tools or documentation for the Debian distribution, like packaging helpers (e.g. debhelper) or integration frameworks (e.g. ca-certificates) and becomes an âupstream authorâ for those creations.
					
						We will not hide problems.
					
						Debian is not perfect, and, there will be new problems to fix every day. Debian will keep its entire bug report database open for public view at all times. Reports that people file on-line will promptly become visible to others.
					
						Our priorities are our users and free software.
					
						This commitment is more difficult to define. Debian imposes, thus, a bias when a decision must be made, and will discard an easy solution for the developers that will jeopardize the user experience, opting for a more elegant solution, even if it is more difficult to implement. This means to take into account, as a priority, the interests of the users and free software.
					
						Works that do not meet our free software standards.
					
						Debian accepts and understands that users may want to use some non-free programs. That is why the project allows usage of parts of its infrastructure to distribute Debian packages of non-free software that can safely be redistributed.
					

COMMUNITY For or against the non-free section?

						The commitment to maintain a structure to accommodate non-free software (i.e. the ânon-freeâ section, see the sidebar [VOCABULARY The main, contrib and non-free archives](apt.en.md#sidebar.sections)) is frequently a subject of debate within the Debian community.
					
						Detractors argue that it turns people away from free software equivalents, and contradicts the principle of serving only the free software cause. Supporters flatly state that most of the non-free packages are ânearly freeâ, and held back by only one or two annoying restrictions (the most common being the prohibition against commercial usage of the software). By distributing these works in the non-free branch, we indirectly explain to the author that their creation would be better known and more widely used if they could be included in the main section. They are, thus, politely invited to alter their license to serve this purpose.
					
						After a first and unfruitful attempt in 2004, the complete removal of the non-free section is unlikely to return to the agenda, especially since it contains many useful documents that were moved simply because they did not meet the new requirements for the main section. This is especially the case for certain software documentation files issued by the GNU project (in particular, Emacs and Make). Many modern systems further also require so called âfirmwareâ, small binary files, which are necessary to run specific hardware. Many of these files can also only be found in the non-free section.
					
						The continued existence of the non-free section is a source of occasional friction with the Free Software Foundation, and is the main reason it refuses to officially recommend Debian as an operating system.
					

### 1.2.2.Â The Debian Free Software Guidelines

				This reference document defines which software is âfree enoughâ to be included in Debian. If a program's license is in accordance with these principles, it can be included in the main section; on the contrary, and provided that free distribution is permitted, it may be found in the non-free section. The non-free section is not officially part of Debian; it is an added service provided to users. A more detailed explanation of the different parts of the Debian archive can be found in the sidebar [VOCABULARY The main, contrib and non-free archives](apt.en.md#sidebar.sections).
			
				More than a selection criteria for Debian, this text has become an authority on the subject of free software, and has served as the basis for the âOpen Source Definitionâ. Historically, it is therefore one of the first formal definitions of the concept of âfree softwareâ.
			
				The GNU General Public License, the BSD License, and the Artistic License are examples of traditional free licenses that follow the 9 points mentioned in this text. Below you will find the text as it is published on the Debian website. âÂ [https://www.debian.org/social_contract.html#guidelines](https://www.debian.org/social_contract.html#guidelines)
Free redistribution.
							The license of a Debian component may not restrict any party from selling or giving away the software as a component of an aggregate software distribution containing programs from several different sources. The license may not require a royalty or other fee for such sale.
						Source code.
							The program must include source code, and must allow distribution in source code as well as compiled form.
						Derived works.
							The license must allow modifications and derived works, and must allow them to be distributed under the same terms as the license of the original software.
						Integrity of the author's source code.
							The license may restrict source code from being distributed in modified form only if the license allows the distribution of âpatch filesâ with the source code for the purpose of modifying the program at build time. The license must explicitly permit distribution of software built from modified source code. The license may require derived works to carry a different name or version number from the original software (This is a compromise. The Debian group encourages all authors not to restrict any files, source or binary, from being modified).
						No discrimination against persons or groups.
							The license must not discriminate against any person or group of persons.
						No discrimination against fields of endeavor.
							The license must not restrict anyone from making use of the program in a specific field of endeavor. For example, it may not restrict the program from being used in a business, or from being used for genetic research.
						Distribution of license.
							The rights attached to the program must apply to all to whom the program is redistributed without the need for execution of an additional license by those parties.
						License must not be specific to Debian.
							The rights attached to the program must not depend on the program being part of a Debian system. If the program is extracted from Debian and used or distributed without Debian but otherwise within the terms of the program's license, all parties to whom the program is redistributed should have the same rights as those that are granted in conjunction with the Debian system.
						License must not contaminate other software.
							The license must not place restrictions on other software that is distributed along with the licensed software. For example, the license must not insist that all other programs distributed on the same medium must be free software.
						

BACK TO BASICS Copyleft

						Copyleft is a principle that consists in using copyrights to guarantee the freedom of a work and its derivatives, rather than restrict the rights of uses, as is the case with proprietary software. It is, also, a play of words on the term âcopyrightâ. Richard Stallman discovered the idea when a friend of his, fond of puns, wrote on an envelope addressed to him: âcopyleft: all rights reversedâ. Copyleft imposes preservation of all initial liberties upon distribution of an original or modified version of a work (usually a program). It is, thus, not possible to distribute a program as proprietary software if it is derived from code from a copyleft released program.
					
						The most well-known family of copyleft licenses is, of course, the GNU General Public License (GPL) and its derivatives, the GNU Lesser General Public License (LGPL), and the GNU Free Documentation License (GFDL). Sadly, the copyleft licenses are generally incompatible with each other. Consequently, it is best to use only one of them.
					Example licenses
							The âGPLâ, âBSDâ, and âArtisticâ licenses are examples of licenses that we consider âfreeâ.
						

BACK TO BASICS Free licenses

						The GNU GPL, the BSD license, and the Artistic License all comply with the Debian Free Software Guidelines, even though they are very different.
					
						The GNU GPL, used and promoted by the FSF (Free Software Foundation), is the most common. Its main feature is that it also applies to any derived work that is redistributed: a program incorporating or using GPL code can only be distributed according to its terms. It prohibits, thus, any reuse in a proprietary application. This poses serious problems for the reuse of GPL code in free software incompatible with this license. As such, it is sometimes impossible to link a program published under another free software license with a library distributed under the GPL. On the other hand, this license is very solid in American law: FSF lawyers have participated in the drafting thereof, and have often forced violators to reach an amicable agreement with the FSF without going to court. âÂ [https://www.gnu.org/copyleft/gpl.html](https://www.gnu.org/copyleft/gpl.html)

						The BSD license is the least restrictive: everything is permitted, including use of modified BSD code in a proprietary application. âÂ [https://www.opensource.org/licenses/bsd-license.php](https://www.opensource.org/licenses/bsd-license.php)

						Finally, the Artistic License reaches a compromise between these two others: integration of code in a proprietary application is permitted, but any modification must be published. âÂ [https://www.opensource.org/licenses/artistic-license-2.0.php](https://www.opensource.org/licenses/artistic-license-2.0.php)

						The complete text of these licenses is available in /usr/share/common-licenses/ on any Debian system (in case of BSD the newer 3-Clause License).
					

COMMUNITY Bruce Perens, a controversial leader

				Bruce Perens was the second leader of the Debian project, just after Ian Murdock. He was very controversial in his dynamic and authoritarian methods. He, nevertheless, remains an important contributor to Debian, to whom Debian is especially indebted for the editing of the famous âDebian Free Software Guidelinesâ (DFSG), an original idea of Ean Schuessler. Subsequently, Bruce would derive from it the famous âOpen Source Definitionâ, removing all references to Debian from it. âÂ [https://opensource.org/](https://opensource.org/)

				His departure from the project was quite emotional, but Bruce has remained strongly attached to Debian, since he continues to promote this distribution in political and economic spheres. He still sporadically appears on the e-mail lists to give his advice and present his latest initiatives in favor of Debian.
			
				Last anecdotal point, it was Bruce who was responsible for inspiring the different âcodenamesâ for Debian versions (1.1 â Rex, 1.2 â Buzz, 1.3 â Bo, 2.0 â Hamm, 2.1 â Slink, 2.2 â Potato, 3.0 â Woody, 3.1 â Sarge, 4.0 â Etch, 5.0 â Lenny, 6.0 â Squeeze, 7 â Wheezy, 8 â Jessie, 9 â Stretch, 10 â Buster, 11 â Bullseye, 12 (not released yet) â Bookworm, Unstable â Sid). They are taken from the names of characters in the Toy Story movie. This animated film entirely composed of computer graphics was produced by Pixar Studios, with whom Bruce was employed at the time that he led the Debian project. The name âSidâ holds particular status, since it will eternally be associated with the Unstable branch. In the film, this character was the neighbor's child who always broke toys â so beware of getting too close to Unstable. Otherwise, Sid is also an acronym for âStill In Developmentâ.
			[PrevChapterÂ 1.Â The Debian Project](the-debian-project.en.md)[Up](#)[Home](index.en.md)[Next1.3.Â The Inner Workings of the Debian Project](sect.debian-internals.en.md)